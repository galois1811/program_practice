## 主要链接
[android for MSM](https://www.codeaurora.org/projects/android-for-msm)  
[wiki(下载指导)](https://wiki.codeaurora.org/xwiki/bin/QAEP/)  
[android releases(含manifest)](https://wiki.codeaurora.org/xwiki/bin/QAEP/release)  
  
    
```bash
$ repo init -u git://codeaurora.org/platform/manifest.git -b release -m [manifest] --repo-url=git://codeaurora.org/tools/repo.git --repo-branch=caf-stable
$ repo sync
```
其中manifest参考androidreleases里面的，855对应的应该是代号为msmnile 的chipset，即manifest使用 LA.UM.7.1.r1-15100-sm8150.0.xml
