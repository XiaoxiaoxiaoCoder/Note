## gopm

---

作用：用户`go get`无法下载翻墙的时候的包，可以用`gopm`下载


### gopm 安装
```
go get github.com/gpmgo/gopm
go install ithub.com/gpmgo/gopm
```

### 使用说明
```
NAME:
   Gopm - Go Package Manager

USAGE:
   Gopm [global options] command [command options] [arguments...]

VERSION:
   0.8.8.0307 Beta

COMMANDS:
   list         list all dependencies of current project
   gen          generate a gopmfile for current Go project
   get          fetch remote package(s) and dependencies
   bin          download and link dependencies and build binary
   config       configure gopm settings
   run          link dependencies and go run
   test         link dependencies and go test
   build        link dependencies and go build
   install      link dependencies and go install
   clean        clean all temporary files
   update       check and update gopm resources including itself
   help, h      Shows a list of commands or help for one command

GLOBAL OPTIONS:
   --noterm, -n         disable color output
   --strict, -s         strict mode
   --debug, -d          debug mode
   --help, -h           show help
   --version, -v        print the version

```

### exmaple
```
gopm get -g -v -u golang.org/x/tools/cmd/goimports
```
