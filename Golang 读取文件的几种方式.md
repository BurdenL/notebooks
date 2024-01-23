# Golang 读取文件的几种方式(ioutil包已废弃，相关接口可以使用io和os包替换，)

## 一、一次性读取全部文件

### 1、使用 os 包配合 ioutil 包读取

```go
package main

import (
    "fmt"
    "io/ioutil"
)

func main() {
    file, err := os.Open("/yourPath/test.txt")
    if err != nil {
        panic(err.Error())
    }
    defer file.Close()
    bytes, err := ioutil.ReadAll(file)
    if err != nil {
        panic(err.Error())
    }
    fmt.Println(string(bytes))
}
```

### 2、仅使用 ioutil 包读取

```go
package main

import (
    "fmt"
    "io/ioutil"
)

func main() {
    bytes, err := ioutil.ReadFile("/yourPath/test.txt")
    if err != nil {
        panic(err.Error())
    }
    fmt.Println(string(bytes))
}
```



## 二、逐行读取文件

### 1、使用 bufio.NewReader 方法

```go
package main

import (
    "bufio"
    "fmt"
    "io"
    "log"
    "os"
)

func main() {
    file, err := os.Open("/yourPath/test.txt")
    if err != nil {
        log.Fatalf("open file failed: %s \n", err.Error())
    }
    defer file.Close()
    reader := bufio.NewReader(file)
    i := 0
    for {
        line, _, err := reader.ReadLine()
        if err == io.EOF {
            break
        }
        i++
        fmt.Printf("line%d: %s \n", i, string(line))
    }
}
```

### xxxxxxxxxx import osimport subprocess​import zipfile​​def find_apk_files(folder_path):    apk_files = []​    # 遍历目标文件夹中的所有文件    for root, dirs, files in os.walk(folder_path):        for file in files:            if file.endswith(".apk"):                apk_file_path = os.path.join(root, file)                apk_files.append(apk_file_path)​    return apk_files​​def find_dex_files(folder_path):    apk_files = []​    # 遍历目标文件夹中的所有文件    for root, dirs, files in os.walk(folder_path):        for file in files:            if file.endswith(".dex"):                apk_file_path = os.path.join(root, file)                apk_files.append(apk_file_path)​    return apk_files​​def unzip_apk(apk_file, destination_folder):    try:        with zipfile.ZipFile(apk_file, 'r') as zip_ref:            zip_ref.extractall(destination_folder)        print(f"APK文件 {apk_file} 已成功解压到 {destination_folder}")    except Exception as e:        print(f"解压APK文件时出错：{e}")​​def run_shell(command):    # 要执行的Shell命令    # command = "adb shell ls -l"  # 以"ls -l"为例，替换为你想要执行的命令​    # 使用subprocess.run执行Shell命令    try:        result = subprocess.run(command, shell=True, stdout=subprocess.PIPE, stderr=subprocess.PIPE,                                text=True)​        # 输出命令的标准输出和标准错误        # print("标准输出：")        # print(result.stdout)        #        # print("标准错误：")        # print(result.stderr)​        # 获取命令的返回代码        return_code = result.returncode        if return_code == 0:            print(f"cmd：{command}\treturn_code：{return_code}")        else:            print(f"cmd：{command}\treturn_code：{return_code}\t ERR: {result.stderr}")​        return result    except Exception as e:        print(f"执行Shell命令时出现错误：{e}")​​if __name__ == '__main__':    current_directory = os.getcwd()    print("当前目录:", current_directory)​    # run_shell(r".\set_jdk17.bat")    run_shell(r".\gradlew.bat :WaterSort:clean")    run_shell(r".\gradlew.bat :WaterSort:assembleRelease")​    target_path = r"./WaterSort/build/outputs/apk"​    apk_list = find_apk_files(target_path)    for apk_file in apk_list:        apk_path = os.path.dirname(apk_file)        zip_path = os.path.dirname(apk_file) + os.path.sep + \                   os.path.splitext(os.path.basename(apk_file))[                       0]        # 使用os.mkdir创建新文件夹        try:            os.mkdir(zip_path)            print(f"已成功创建文件夹：{zip_path}")        except OSError as e:            print(f"创建文件夹时出错：{e}")        print("apk_file: " + apk_file)        print("apk_path: " + apk_path)        print("zip_path: " + zip_path)        unzip_apk(apk_file, zip_path)        dex_path = zip_path + os.path.sep + r"classes.dex"​        # 解压apk文件，并记录dex文件的地址        result = run_shell(r"adb devices")        if result.returncode == 0:            run_shell(r"adb shell rm -rf /sdcard/hook/hook")            run_shell(fr"adb push {dex_path} /sdcard/hook/hook")python

```go
package main

import (
    "bufio"
    "fmt"
    "log"
    "os"
)

func main() {
    file, err := os.Open("/yourPath/test.txt")
    if err != nil {
        log.Fatalf("open file failed: %s \n", err.Error())
    }
    defer file.Close()
    scanner := bufio.NewScanner(file)
    i := 0
    for scanner.Scan() {
        line := scanner.Text()
        i++
        fmt.Printf("line%d: %s \n", i, string(line))
    }
}
```

## 三、按字节读取文件

### 1、使用 [file.Read](http://file.read/) 每次读取固定字节

```go
package main

import (
    "fmt"
    "io"
    "log"
    "os"
)

func main() {
    file, err := os.Open("/yourPath/test.txt")
    if err != nil {
        log.Fatalf("open file failed: %s \n", err.Error())
    }
    defer file.Close()
    buf := make([]byte, 1024)
    bytes := make([]byte, 0)
    for {
        // 如果存在中文此方式可能出现乱码
        size, err := file.Read(buf)
        if err == io.EOF || size == 0 {
            break
        }
        bytes = append(bytes, buf[:size]...)
        // 加入分隔符验证
        // bytes = append(bytes, []byte("###")...)
    }
    fmt.Println(string(bytes))
}
```

### 2、使用 [bufio.Rad](http://bufio.read/) 每次读取固定字节

```text
package main

import (
    "bufio"
    "fmt"
    "io"
    "log"
    "os"
)

func main() {
    file, err := os.Open("/yourPath/test.txt")
    if err != nil {
        log.Fatalf("open file failed: %s \n", err.Error())
    }
    defer file.Close()
    reader := bufio.NewReader(file)
    buf := make([]byte, 50)
    bytes := make([]byte, 0)
    for {
        // 如果存在中文此方式可能出现乱码
        size, err := reader.Read(buf)
        if err == io.EOF || size == 0 {
            break
        }
        bytes = append(bytes, buf[:size]...)
        // 加入分隔符验证
        // bytes = append(bytes, []byte("###")...)
    }
    fmt.Println(string(bytes))
}
```

### 3、使用 bufio.ReadByte 每次读取一个字节

```go
package main

import (
    "bufio"
    "fmt"
    "log"
    "os"
)

func main() {
    file, err := os.Open("/Users/van/WorkSpace/go/learngo/fileOperation/file/ID2Path_size.txt")
    if err != nil {
        log.Fatalf("open file failed: %s \n", err.Error())
    }
    defer file.Close()
    reader := bufio.NewReader(file)
    bytes := make([]byte, 0)
    for {
        byte, err := reader.ReadByte()
        if err != nil {
            break
        }
        bytes = append(bytes, byte)
        // fmt.Printf("%s \n", string(byte))
    }
    fmt.Printf("content: %s \n", string(bytes))
}
```

### 4、使用 bufio.ReadRune 每次读取一个字符

```go
package main

import (
    "bufio"
    "fmt"
    "log"
    "os"
)

func main() {
    file, err := os.Open("/yourPath/test.txt")
    if err != nil {
        log.Fatalf("open file failed: %s \n", err.Error())
    }
    defer file.Close()
    reader := bufio.NewReader(file)
    runes := make([]rune, 0)
    for {
        readRune, size, err := reader.ReadRune()
        if err != nil || size == 0 {
            break
        }
        runes = append(runes, readRune)
        // fmt.Printf("%d - %s \n", size, string(readRune))
    }
    fmt.Printf("content: %s \n", string(runes))
}
```

## 四、按分隔符读取文件

### 1、 bufio.ReadBytes

```go
package main

import (
    "bufio"
    "fmt"
    "log"
    "os"
)

func main() {
    file, err := os.Open("/yourPath/test.txt")
    if err != nil {
        log.Fatalf("open file failed: %s \n", err.Error())
    }
    defer file.Close()
    reader := bufio.NewReader(file)
    bytes := make([]byte, 0)
    for {
        bytesTmp, err := reader.ReadBytes('\n')
        if err != nil {
            break
        }
        bytes = append(bytes, bytesTmp...)
        // fmt.Printf("%s \n", string(bytesTmp))
    }
    fmt.Printf("content: %s \n", string(bytes))
}
```

### 2、bufio.ReadSlice

```go
package main

import (
    "bufio"
    "fmt"
    "log"
    "os"
)

func main() {
    file, err := os.Open("/yourPath/test.txt")
    if err != nil {
        log.Fatalf("open file failed: %s \n", err.Error())
    }
    defer file.Close()
    reader := bufio.NewReader(file)
    bytes := make([]byte, 0)
    for {
        line, err := reader.ReadSlice('\n')
        if err != nil {
            break
        }
        bytes = append(bytes, line...)
        // fmt.Printf("%s \n", string(line))
    }
    fmt.Printf("content: %s \n", string(bytes))
}
```

### 3、bufio.ReadString

```go
package main

import (
    "bufio"
    "fmt"
    "log"
    "os"
)

func main() {
    file, err := os.Open("/yourPath/test.txt")
    if err != nil {
        log.Fatalf("open file failed: %s \n", err.Error())
    }
    defer file.Close()
    reader := bufio.NewReader(file)
    var content string
    for {
        str, err := reader.ReadString('\n')
        if err != nil {
            break
        }
        content += str
        // fmt.Printf("%s \n", str)
    }
    fmt.Printf("content: %s \n", content)
}
```