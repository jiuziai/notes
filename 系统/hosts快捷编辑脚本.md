# hosts快捷编辑脚本
> 一个用go编写的快速编辑hosts文件的命令行工具，可根据域名从ipaddress.com中获取ip，在hosts文件中添加或者修改。
---
## 使用  

1. 根据hosts文件位置修改变量```hostsPath```  

2. 安装依赖包：  
    ```go get -u github.com/PuerkitoBio/goquery```  

3. 解决项目依赖：  
    ```go mod tidy```  

4. 构建项目，生成可执行文件：  
    ```go build```
---
## 代码
```go
package main

import (
	"bufio"
	"flag"
	"fmt"
	"net/http"
	"os"
	"regexp"
	"strings"

	"github.com/PuerkitoBio/goquery"
)

// 根据系统修改hosts文件位置
// var hostsPath = "/etc/hosts"
var hostsPath = "C:\\Windows\\System32\\drivers\\etc\\hosts"
var (
	a string
	d string
	i string
	l bool
)

func main() {
	flag.BoolVar(&l, "l", false, "查看现有hosts列表")
	flag.StringVar(&a, "a", "", "根据域名增加hosts中一条解析\n(有则更新，无则添加)\neg: -a test.localhost")
	flag.StringVar(&d, "d", "", "根据域名删除hosts中一条解析\neg: -d test.localhost")
	flag.StringVar(&i, "i", "", "增加hosts解析时指定域名的IP地址\neg: -a test.localhost -i 127.0.0.1")
	flag.Parse()
	if flag.NFlag() > 2 {
		flag.CommandLine.Usage()
		return
	}
	if flag.NFlag() > 1 {
		if a == "" || i == "" {
			flag.CommandLine.Usage()
			return
		}
	}
	if flag.NFlag() == 0 {
		if a == "" || i == "" {
			flag.CommandLine.Usage()
			return
		}
	}
	if l {
		traversalFile(func(f *os.File, r *bufio.Scanner) {
			for r.Scan() {
				foundAnnotation, _ := regexp.MatchString("^\\s*^#", r.Text())
				foundBlankLine, _ := regexp.MatchString("^\\s+$", r.Text())
				if !foundAnnotation && !foundBlankLine {
					fmt.Println(r.Text())
				}
			}
		})
	}
	if a != "" {
		traversalFile(func(f *os.File, r *bufio.Scanner) {
			context := ""
			exist := false
			ip := ""
			var err error
			for r.Scan() {
				foundBlankLine, _ := regexp.MatchString("\\S", r.Text())
				if foundBlankLine {
					foundDomain, _ := regexp.MatchString("\\s+"+a+"\\s*$", r.Text())
					if foundDomain {
						exist = true
						ip, err = getSiteIp(a)
						if err != nil {
							fmt.Println(err)
							break
						}
						context += ip + " " + a + "\n"
					} else {
						context += strings.TrimSpace(r.Text()) + "\n"
					}
				}
			}
			if !exist {
				ip, err = getSiteIp(a)
				if err != nil {
					fmt.Println(err)
					return
				}
				context += ip + " " + a + "\n"
			}
			err = f.Truncate(0)
			if err != nil {
				fmt.Println(err)
				return
			}
			_, err = f.Seek(0, 0)
			if err != nil {
				fmt.Println(err)
				return
			}
			_, err = f.WriteString(context)
			if err != nil {
				fmt.Println(err)
				return
			}
			if exist {
				fmt.Printf("更新%s解析成功，%s的IP为：%s！\n", a, a, ip)
			} else {
				fmt.Printf("添加%s解析成功，%s的IP为：%s！\n", a, a, ip)
			}

		})
	}
	if d != "" {
		traversalFile(func(f *os.File, r *bufio.Scanner) {
			context := ""
			exist := false
			for r.Scan() {
				foundBlankLine, _ := regexp.MatchString("\\S", r.Text())
				if foundBlankLine {
					foundDomain, _ := regexp.MatchString("\\s+"+d+"\\s*$", r.Text())
					if foundDomain {
						exist = true
					} else {
						context += strings.TrimSpace(r.Text()) + "\n"
					}
				}
			}
			err := f.Truncate(0)
			if err != nil {
				fmt.Println(err)
				return
			}
			_, err = f.Seek(0, 0)
			if err != nil {
				fmt.Println(err)
				return
			}
			_, err = f.WriteString(context)
			if err != nil {
				fmt.Println(err)
				return
			}
			if exist {
				fmt.Printf("删除%s成功！\n", d)
			} else {
				fmt.Printf("%s在hosts中不存在！\n", d)
			}
		})
	}
}

func traversalFile(f func(f *os.File, r *bufio.Scanner)) {
	var file *os.File
	var err error
	// hosts文件位置
	file, err = os.OpenFile(hostsPath, os.O_RDWR, 0666)
	if err != nil {
		fmt.Println(err)
		return
	}
	defer file.Close()
	reader := bufio.NewScanner(file)
	f(file, reader)
}

func getSiteIp(s string) (string, error) {
	url := fmt.Sprintf("https://sites.ipaddress.com/%s", s)
	response, err := http.Get(url)
	if err != nil {
		return "", err
	}
	defer response.Body.Close()
	body, err := goquery.NewDocumentFromReader(response.Body)
	if err != nil {
		return "", err
	} else {
		return body.Find("#tabpanel-dns-a pre a").First().Text(), nil
	}
}

```