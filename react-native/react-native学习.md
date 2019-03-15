## 问题列表
+ 使用官方方式创建项目，<code>npm start</code> 执行错误

```shell
ERROR  watchman--no-pretty get-sockname returned with exit code null dyld: Library not loaded: /usr/local/lib/libpcre.1.dylib
  Referenced from: /usr/local/bin/watchman
  Reason: image not found

Error: watchman--no-pretty get-sockname returned with exit code null dyld: Library not loaded: /usr/local/lib/libpcre.1.dylib
  Referenced from: /usr/local/bin/watchman
  Reason: image not found
```

```shell
TT:proj fish$ brew uninstall watchman
Uninstalling /usr/local/Cellar/watchman/3.2.0... (6 files, 200K)
TT:proj fish$ brew install --HEAD watchman
Error: You must `brew link pcre' before watchman can be installed
TT:proj fish$ brew link pcre
Linking /usr/local/Cellar/pcre/8.37...
Error: Could not symlink share/man/man3/pcre.3
/usr/local/share/man/man3 is not writable.
TT:proj fish$ whoami
fish
TT:proj fish$ sudo chown -R `whoami` /usr/local
#OK
```
参考文档：  
https://github.com/Homebrew/homebrew/issues/9953#issuecomment-3800557  
http://stackoverflow.com/questions/29319378/cant-link-pcre-thru-brew-in-max-os-yosemite  
【官方】http://facebook.github.io/react-native/docs/troubleshooting.html  

+ 找不到 bundle文件

```shell
1.Open iOS/AppDelegate.m
2.Comment out jsCodeLocation = [NSURL URLWithString:@"http://localhost:8081/index.ios.bundle"];
3.Uncomment jsCodeLocation = [[NSBundle mainBundle] URLForResource:@"main" withExtension:@"jsbundle"];
4.Start up the packager with npm start and then run curl http://localhost:8081/index.ios.bundle -o main.jsbundle - if this fails (as it did on my machine) add the --ipv4 flag to the end of it.
5.In XCode, right click on your project directory and click Add Files to "Project Name Here" - choose the main.jsbundle file that you generated.
````

参考：    
https://github.com/facebook/react-native/issues/240#issuecomment-86977126    
  
  
  
  
  
