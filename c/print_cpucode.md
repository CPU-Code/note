# 第一条Linux\_c输出

初界面 :

![image-20200727152454725](https://gitee.com/cpu_code/picture_bed/raw/master//20200727152501.png)

```text
ls    # 显示目录下的文件
```

![image-20200727152541225](https://gitee.com/cpu_code/picture_bed/raw/master//20200727152541.png)

```text
cd    # 进入到某个目录
# 比如 我进入了Code
```

![image-20200727152630504](https://gitee.com/cpu_code/picture_bed/raw/master//20200727152630.png)

```text
ls # 发现没有显示, 说明为文件下为空
```

![image-20200727152728637](https://gitee.com/cpu_code/picture_bed/raw/master//20200727152728.png)

```text
vim cpucdoe.c    # 创建一个 .c的源码文件
```

![image-20200727152846782](https://gitee.com/cpu_code/picture_bed/raw/master//20200727152846.png)

进入到了vim的编辑界面:

![image-20200727152933878](https://gitee.com/cpu_code/picture_bed/raw/master//20200727152933.png)

```text
i    # 按i 就可以进行编辑 , 下面显示插入标识
```

![image-20200727153000441](https://gitee.com/cpu_code/picture_bed/raw/master//20200727153000.png)

在编辑模式下, 可以通过, 上下左右进行控制

或

```text
esc    # 在编辑模式下按, 就进入浏览模式
```

```text
h    # 左
l    # 右
k    #上
j    # 下
```

利用这个进行编辑 .c源码

```c
#include <stdio.h>

int main()
{
    printf("cpucode!");

    return 0;
}
```

编写完后 :

```text
:wq        #保存并退出
```

![image-20200727153931991](https://gitee.com/cpu_code/picture_bed/raw/master//20200727153932.png)

按回车就退出了

```text
ls    # 查看文件
```

![image-20200727154356165](https://gitee.com/cpu_code/picture_bed/raw/master//20200727154356.png)

```text
gcc cpucode.c    # 编译c文件
```

```text
ls    # 可以看见生成文件 a.out
```

![image-20200727154452471](https://gitee.com/cpu_code/picture_bed/raw/master//20200727154452.png)

```text
./a.out    # 执行生成文件, 就可以看见输出输出结果了
```

![image-20200727154621365](https://gitee.com/cpu_code/picture_bed/raw/master//20200727154621.png)

ubuntu切换中文时报software database is broken错误。

网上的办法千篇一律，还都没有用。都是去应用中心删除thundbird之类的，啊。。。。。。。

在终端下执行

sudo apt-get install language-pack-zh-han\*

然后去语言中心设置中文，并全局使用即可。

解决方法： sudo apt-get remove language-pack-zh-hans-base

