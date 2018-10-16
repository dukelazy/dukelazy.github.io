---
layout: post
title: "Windows外壳名字空间的浏览"
date: 2016-05-26 17:43:33 +0800
comments: true
categories: 
---

Windows95/98对Dos/Win3.x作了许多重大改进，在文件系统方面，它除了采用长文件名替代Dos中的8.3文件名以外，引入外壳名字空间（Shell Name Space）来代Dos文件系统是其又一大突破．本文将简要地介绍如何在Windows 95/98或Windows NT4.0以上版本。

<!--more-->

## 一、概述

### 1.1 简介
在Dos/Win3.x中，每个逻辑分区构成一棵目录树，文件系统由这一统一的根，而且每个目录或文件必须一一对应于文件系统中客观存在的项。但Windows引入了“外壳名字空间”（ Shell Name Space）的概念之后，这一切就都变了。

外壳名字空间是Windows下的标准文件系统，它大大扩展了Dos文件系统，形成了以“桌面”（Desktop）为根的单一的文件系统树，原有的Ｃ盘、Ｄ盘等目录树变成了“我的电脑”这一外壳名字空间子树的下一级子树，而像“控制面板”、“回收站”、“网上邻居”等应用程序及“打印机”等设备也被虚拟成了外壳名字空间中的节点。另外，与ＤＯＳ中物理存储只能和文件系统项一一对应这一点不同的是，一个实际目录在外壳名字空间中可以表现为不同的项。例如“我的文档”与“C;/My Documents”其实都指向“C;/My Documents”目录，但它们在外壳名字空间中是不同的项。如果我们运行Windows 自带的“Windows资源管理器”看一下的话，那么在它的左部树型视图中我们就可以清楚的看到整个外壳名字空间替代ＤＯＳ文件系统，Windows在文件系统的组织与管理上终于有了质的飞跃。

为了区别于ＤＯＳ中“目录”的概念，Windows引入了“文件夹”（Folder）的概念。“文件夹”一般是指外壳名字空间树中的非叶节点，既可以是ＤＯＳ下的目录，也可是“控制面板”、“回收站”这类虚拟的目录。但外壳名字空间中有些项本身并不是文件夹（即不具有文件夹属性），但却含有子文件夹，比如“网上邻居”等。以下为讲座方便，我们也认为它们是文件夹。

在下面的讲座过程中我们将用“文件系统”一词来指代ＤＯＳ文件系统，而用“外壳名字空间”一词来指代Windows中的外壳名字空间：另外用“文件”一词来指代外壳名字空间这棵树中的叶节点（虽然它们不都是物理存储上的文件）。

在Windows中，Win3.x的文件操作函数，如FindFirstFile、FindNextFile、SetCurrentDirectory等，虽然仍可使用，但用它们只能浏览文件系统，却无法浏览与操纵整个外壳名字空间。要浏览Windows中的外壳名字空间，就必须使用一套全新的、基于ＣＯＭ（组件对象模型）基础上的方法。

### 1.2 新的“路径”PIDL

在讨论基于 COM 的方法之前，我们先来介绍一下外壳名字空间中“路径”的表示问题。DOS 的字符串路径只能表示文件系统，而无法表示整个外壳名字空间，所以外壳名字空间提供了一种“路径”的替代 PIDL。

PIDL 是一个元素类型为 ITEMIDLIST 结构的数组，数组中元素的个数是未知的，但紧接着数组末尾的必是一个双字节的零。每个数组元素代表了外壳名字空间树中的一层（即一个文件夹或文件），数组中的前一元素代表的是后一元素的父文件夹。由此可见，PIDL 实际上就是指向一块由若干个顺序排列的 ITEMIDLIST 结构组成、并在最后有一个双字节零的空间的指针。所以 PIDL 的类型就被 Windows 定义为 ITEMIDLIST 结构的指针。

PIDL亦有“绝对路径”与“相对路径”的概念。表示“相对路径”的PIDL（本文简称为“相对PIDL”）只有一个ITEMIDLIST结构的元素，用于标识相对于父文件夹的“路径”；表示“绝对路径”的PIDL（简称为“绝对PIDL”）有若干个ITEMIDLIST结构的元素，第一个元素表示外壳名字空间根文件夹（“桌面”）下的某一子文件夹A，第二个元素则表示文件夹A下的某一子文件夹B，其余依此类推。这样绝对PIDL就通过保存一条从“桌面”下的直接子文件夹或文件的绝对PIDL与相对PIDL是相同的，而其他的文件夹或文件的相对PIDL就只是其绝对PIDL的最后一部分了。

但现在就出现了一个问题：即“桌面”的表示问题。外壳名字空间中其他各项都可以用从“桌面”开始的绝对PIDL加以表示，但“桌面”的PIDL数组显然一个元素都没有。这样就只剩下PIDL数组最后的那个双字节的零了。所以，“桌面”的PIDL就是一个16位的零。注意：“桌面”内容是一个双字节的零。另外，虽然“桌面”表示的是“C:/Windows/Desktop”文件夹（这里假定Windows的系统目录为“C:/Windows”）的内容，但“桌面”与“C:/Windows/Desktop”文件夹的PIDL是完全不同的。这一点同样适用于“我的文档”与“C:/My Documents”等文件夹。

DOS中的路径是一个字符串，但PIDL是一种二进制结构，所以我们不能直接从PIDL中获知它所代表的到底是哪个文件夹或文件，而必须调用相应的函数把它转换成代表路径的字符串。如果某绝对PIDL是文件系统的一部分，则调用SHGetPathFromIDList函数即可；但如不是，就无法获得路径字符串了，因为DOS中根本就不存在这种路径。但很可惜的是，Windows并没有提供一个函数来让我们方便地把文件系统的路径字符串转换成PIDL。不过我们可用一个我们自己实现的函数 ParsePidlFromPath() 来达目的（具体函数的实现见下文）。

PIDL的创建与释放一般并不使用C++的new和delete操作或C语言的malloc和free函数,而必须使用专门的方法进行.首先调用SHGEetMallocI函数得到Malloc接口(COM接口的一种,关于COM接口下面将详述)的指针,再调用该接口的Alloc方法为PIDL分配空间,或调用该接口的Free方法释放某个PIDL占用的空间。最后调用该接口的Release方法释放该接口。

除了下面将要介绍的IShellFolder、IEnumIDList等COM接口可以操作PIDL外，还有很多以SH开头的Windows API函数也可操作PIDL，不过一般这些函数都要求使用绝对PIDL作参数。例如SHGetFileInfo函数可得到某一PIDL所指对象的各种信息，包括名字、图标、属性等；SHFileOperation函数可对外壳名字空间中的项进行拷贝、移动、改名、删除等操作；SHBrowseForFolder可以显示一个让用户选择外壳名字空间中某一文件夹的浏览对话框.

### 1.3 基于COM的方法

讨论清楚了PIDL的概念之后,我们回过头来讨论基于COM之上的浏览外壳名字空间的方法。如果说PIDL是外壳的名字空间中的“路径”的话，那么下面所说的两个COM接口IshellFolder与IEnumIDList就起着与Win 3.x中的FindFirstFile、FindNextFile等函数类似的功能。

在Windows中，每个文件夹都由操作系统实现了一个派生自Iunknown接口（COM接口的最基本类）的接口IshellFolder。通过调用某个文件夹的该接口，即可实现对该文件夹的浏览，得到该文件夹中子项（子文件夹或文件）的各种相关信息。

我们可以调用SHGetDesktopFolder函数来获得外壳名字空间的根文件夹（即“桌面”）的IshellFolder接口。对于某个文件夹A，以它的子文件夹B的相对PIDL为参数，调用它的IshellFolder接口的BindToObject方法即可得到子文件夹B的IshellFolder接口。如要枚举某个文件夹下的子项，则只需调用它的IshellFolder接口的EnumObjects方法即可获得一个IEnumIDList接口。通过调用该IEnumIDList接口的Next方法我们即可枚举出该文件夹的所有子项（包括文件夹和文件等对象），获得它们的相对PIDL。使用父文件夹的IshellFolder接口和这些相对PIDL，我们即可获得这些子项的各种相关信息，包括显示名称、图标、属性等，甚至还可以获得它的右键菜单。例如，调用该接口的GetDisplayNameOf方法可获得该文件夹下子项的显示名称；调用ParseDisplayName方法可把某个子项的用Unicode内码表示的字符串路径翻译成对应的PIDL。这样通过PIDL和这两个接口，我们就可以遍历和操纵整个外壳名字空间了。

除了IshellFolder和IEnumIDList接口以外，Windows 外壳名字空间还提供了很多其他COM接口，例如IshellBrowser、IshellLink、IshellIcom、IshellView等。通过这些接口，应用于程序就可以更好的与外壳名字空间交互。由于本文篇幅有限，这些接口就不详细介绍了，有兴趣的读者可参阅相关资料。

值得注意的是，COM中的接口虽然在使用上与C++中的类是非常相似（事实上COM接口在C ++中就是以类的形式声明的），但维护其正确的引用计数机制是非常重要的。每增加一个对该接口的引用，就要调用一次它的AddRef( )方法；而在使用完后必须调用它的Release( )方法释放该接口。关于COM及COM接口的细节请参见相关资料，这里不再赘述。

可惜的是，虽然我们可依照上文给出的方法实现外壳名字空间的逐层展开，但外壳名字空间却并没有提供一种让我们自由跳转到某一文件夹的方法，也没有提供返回到上一级文件夹的方法，因为我们无法方便地获得父文件夹的IshellFolder接口。如果要返回，就必须由应用程序自己想方法获得父文件夹的IshellFolder接口。一种可行的方法是在展开外壳名字空间时保存每个文件夹的IShellFolder接口指针和它的绝对PIDL，这样就可以相对容易地实现自由跳转了。

但无论如何，外壳名字空间提供的浏览和操作的方法比起DOS/ Windows 3.x的函数来还是有着巨大的飞跃的。只要我们理解清楚了这种方法的优点与不足，我们就可以扬长避短，开发出各种各样的使用外壳名字空间的程序来。

## 二、相关接口、函数和数据结构

对于本文所涉及的一些比较复杂的接口、函数和数据结构，以下仅列举出作者在Visual C++6.0查到的声明与定义,并配上相应的注释.一些较简单的则从略,未列出的请参见相关资料。

### 2.1 数据结构
``` cpp
typedef IshellFolder *LPSHELLFOLDER;
//IshellFolder接口指针的声明

typedef struct _ITEMIDLIST {//ITEMIDLIST结构的定义
    SHITEMID mkid;
}ITEMIDLIST, *LPITEMIDLIST;

typedef struct _SHITEMID {//ITEMIDLIST结构中元素的定义
    USHORT cb;//本结构的长度(以字节计)
    BYTE abID[1];//可变长的元素标识符
}SHITEMID, *LPSHITEMID;

typedef struct _SHFILEINFO {//SFFILEINFO结构的定义
    HICON hicon;//文件图标的句柄
    Int ilcon;//图标在系统图像列表中的序号
    DWORD dwAttributes;//文件的属性
    Char szDisplayName[MAX_PATH];//显示名称或路径
    Char szTypeName[80];//表示文件类型的字符串
} SHFILEINFO;
```

### 2.2 相关接口
#### 2.2 IshellFolder接口的方法

- 2.1.1 BindToObject
    + 格式: `HRESULT BindToObject( LPCITEMIDLLIST pidl, LPBC pbcreserved, REFIID riid, LPVOID *ppvOut);`
    + 作用：得到本文件夹中某一子文件夹的IShellFolder接口。
    + 参数：Riid应为IID_IshellFolder, pbcReserved应为NNUL,pidl为表示该子文件夹的“相对路径”的PIDL,从ppvOut中返回要求的IshellFolder接口的指针。
- 2.1.2 EnumObjects
    + 格式：'HRSULT EnumObjects( HWND hwndOwner, DWORD grfFlags, LPENUMIDLIST*ppenumIDList);'
    + 作用：枚举本文件夹的成员。
    + 参数：hwndOwmer为父窗口句柄，grfFlags决定枚举世闻名的内容，可为SHCONTF_FOLDERS、SHCONTF_NONFOLDERS、SHCONTF_INCLUDEHIDDEN的组合,从ppenumIDList返回IEnumIDList接口的指针。
- 2.1.3 GetDisplayNameOf
    + 格式：'HRESULT GetDisplayNameOf (LPCITEMIDLIST pidl, DWORD uFlags, LPSTRRERT lpName);'
    + 作用：得到本文件夹中某一对象的显示名称。
    + 参数：pidl为表示该子文件夹的“相对路径”的PIDL, uFlags为 SHGDN_NORMAL、SHGDN_INFOLDER、SHGFI_SYSICONINDEX、SHGFI_EXETYPE、SHGFI_ATTRIBUTES、SHGFI_PIDL、SHGFI_DISPLAYNAME、SHGFI_LARGEICON等。
    + 返回值：如uFlags包含SHGFI-EXETYPE标志，则返回值为该可执行文件夹类型；如uFlags包含SHGFI_SYSICONINDEX标志,则返回值为系统图像列表的句柄。否则,如本函数调用成功则返回非零值，失败则返回零。

## 三、应用举例

几个非常有用的函数的实现

### 3.1 ParsePidlFromPath
描述：将文件系统路径翻译成对应的PIDL。
``` cpp
LPITEMIDLIST ParsePidlFromPath(LPCSTR path)
{
    //存放以Unicode内码表示的路径字符串的缓冲区
    OLECHAR szOleChar[MAX_PATH];
    //“桌面“的IshellFolder接口指针
    LPSHELLFOLDER IpsfDeskTop;
    //返回的PIDL
    LPITEMIDLIST Ipifq;
    ULONG ulEaten, ulAttribs;
    HRESULT hres;
    //得到“桌面”的IshellFolderr 接口指针
    SHGetDesktopFolder(&lpsfDeskTop);
    //将Ansi字符集的路径字符串转换成Unicode字符串，  存入szOleChar
    MultiByteToWideChar(CP_ACP, MB_PRECOMPOSED, Path, -1, szOleChar, sizeof(szOleChar));
    //将szOleChar,中的路径径字符串翻译成相应的PIDL，存入lpifq
    hres = lpsfDeskTop->Release();
    //如果翻译失败，则返回NULL
    if (FAILED(hres))
        return NULL;
    return lpifq;
}
```

### 3.2 GetItemIcon

描述：返回lpi这个绝对PIDL所指项的图标在系统图像列表中的序号，uFlags为要求的图标类型。
``` cpp
Int Getltemlcon(LPITEMIDLIST lpi, UINT uFlags)
{
    //存放文件信息的结构
    SHFILEINFO sfi;
    //给uFlags增加一些公共标志（lpi为PIDL、要求返回系统图像列表、要求小图标）
    uFlags |= SHGFI - PIDL | SHGFI_SYSICONINDEX | SHGFI_SMALLICON;
    //获得图标
    SHGetFileinfo((LPCSTR)lpi, 0, &sfi, sizeof(SHFILEINFO), uFlags);
    //返回图标在系统图像列表中的序号KD
    return sfi, ilcon;
}
```

###3.3 GetName
描述：lpio lpsf所指的IshellFolder接口代表的文件夹下的相对PIDL，本函数获得lpi所指项的显示名称，dwFlags表明欲得到的显示名称类型，lpFriendlyName为存放显示名称的缓冲区。
``` cpp
BOOL GetName(LPSHELLFOLDER lpsf, LPITEMIDLIST lpi, DWORD dwFlags, LPSTR lpFriendlyName)
{
    STRRET str;
    //得到显示名称
    if (NOERROR != lpsf->GetDisplayNameOf()lpi, dwFlags, &str))
    return FALSE;
    //根据返回值进行转换
    switch (str uType)
    {
        //如为Unicode字符串，则转成Ansi字符集的字符串case STRRET_WSTR:
        WideCharToMultiByte(CP_ACP, 0, str.pOleStr, -1, ipFriendlyName, sizeof(lpFriendlyName), NULL, NULL);
        Break;
        //如为偏移量，则去除偏移量
    case STRRET_OFFSET:
        lstrcpy(lpFriendlyName, (LPSTR)lpi + str.uOffset);
        break;
        如为Ansi字符串，则直接拷贝
    case STRRET_CSTR:
        Lstrcpy(lpFriendlyName, (LPSTR)str.cStr);
        Break;
        //非法情况
    default:
        return FALSE;
    }
    return TRUE;
}
```

## 四、一个实例 

以下我们将用Visual C++6.0制作一个例子来演示外壳名了空间的浏览。具体为使用Ctreer View,展开外壳名字空间中的“桌面”文件夹，枚举出该文件夹下的所有子文件夹。

在这个项目中，CtreeView 的图像列表我们使用Windows 的系统图像列表，而不是自己创建一个。

首先，用AppWizard新建一个项目，类型为MFC AppWizard(exe),项目名为Test；在第一步中选择Single document;在第六步中将CtestView的基类改为CtreeView。其它均使用默认设置。

其次，在CtestView中加一个私有成员变量m_ImageList,类型为CimageList,用于保存系统列表。（Windows中所有的图标都保存在系统图像列表中，我们可以在程序中得到这个图像列表）。

第三步，将上文提到的GetName 和GetItemIcon 这两个函数的实现拷贝到CtestView.cpp的较开头的位置。

第四步，在CtestView的OnInitialUpdate( )函数中加入以下代码：
``` cpp
//系统图像列表的句柄
HIMAGELIST himlSmall;
//存放文件信息的结构
SHFILEINFO sfi;
//存放树型控件中的节点的信息
TV_INEM tvi;
//向树型控件中插入节点时使用的结构
TV_INSERTSTRUCT tvis;
//欲插入节点的前一节点的句柄
HTREEITEM hParent = TVI_FIRST;
//欲节点的父节点的句柄
HTREEITEM hParent = TV_ROOT;
//某一文件夹的IshellFolder接口指针
LPSHELLFOLDER lpsf = 0;
//IenumiDList接口的指针
LPENUMIDLIST lpe = 0;
//lpi为一PIDL
LPITEMIDLIST lpi = 0;
//IMalloc接口的指针
LPMALLOC lpMalloc = 0;
//枚举的个数
ULONG ulFetched;
//存放显示名称的缓冲区
char szBuff[MAX_PATH];
//获得系统图像列表，并把它赋给CtestView的CtreeCtrl控件
himlsmall = (HIMAGELIST)SHGetFileinfo(“C://”,0,&
sfi, sizeof(SHFIEINFO), SHGFI_SYSICONINDEX | SHGFI_SMALLICON);
m_lmageList.Attach(himlsmall);
GetTreeCtrl().SetlmageList(&m_imageList, TVSIL_NORMAL);
//获得Imalloc接口的指针
SHGetMalloc(&lpMalloc);
//获得“桌面”文件夹的IshellFolder接口指针
SHGetDesktopFloder(&lpsf);
//创建一个“桌面”的绝对PIDL
lpi = (LPITEMIDLIST)lpMalloc->Alloc(sizeof(USHORT0));
*((USHORT*)lpi) = 0
    //设置要插入的树节点信息
    tvi, mask = TVIF_TEXT | TVIF_IMAGE | TVIF_SELECTEDIMAGE | TVIF_CHILDREN;
tvi.cchTextMax = MAX_PATH;
//设置显示名称
tvi.pszText = _T（“桌面”）；
    //获得标准图标和展开时的图标
    tvi.ilmage = Tetltemlcon(lpi, NULL);
tvi.iSelectedlmage = Getltemlcon(lpi, SHGFI_OPENICON);
//设置插入位置
tvis.item = tvi;
tvis.hlnsertAfter = TVI_FIRST;
tvis.hParent = TVI_ROOT;
//插入根节点
hpParent = GetTreeCtrl().Instrtltem(&tvis);
//释放lpi所占的空间
lpMalloc->Free(lpi);
//获得“桌面”文件夹的IenumiDList接口指针lpe
lpsf->EnumObjects(m_hWnd, SHCONTF_FOLDERS |
    SHCONTF_NONFOLDERS, &lpe);
//枚举“桌面”下的各个子文件夹
while (S_OK = = lpe->Next(1, &lpi, &ulFetched))
{
    //获得lpi表示的子文件夹的显示名称
    GetName(lpsf, lpi, SHGDN_NORMAL, szBuff);
    tvi.pszText = szBuff;
    // 获得该项的图标
    //由于是“桌面”下的直接子项，所以它的相对PIDL与绝对PIDL是一致的
    tvi.ilmage = Getltemlcon(lpi, NULL);
    tvi.iSelectedimage = Getltemlcon(lpi, SHGFI_OPENICON);
    //设置插入位置
    tvis.item = tvi;
    tvis.hinsertAfter = hPrev;
    tvis.hParent = hParent;
    //插入节点
    hPrev = GetTreeCtrl().insertltem(&tvis);
    //释放lpi所占的空间
    ipMalloc->Free(lpi);
}
//释放Imalloc和IsshellFolder接口
lpMalloc->Release();
lpsf->Release();
//对生成的节点进行排序
GetTreeCtrl().SortChildren(hParent);
//将CtestView中的“桌面”节点展开
GetTreeCtrl().Selectltem(hParent);
GetTreeCtrl().Expand(hParent, TVE_EXPAND);
```
最后，响应CtestView的WM_DESTROY消息，加入以下代码：
``` cpp
//由于使用了系统图像列表，退出时必须释放对它的所有权
//否则，退出后Windows将一个图标没有
m_imageList.Detach( );
```
这个演示程序的效果如下图所示：

## 五、后记

由于篇幅的关系，本文所举的例子只能非常简单的演示一下外壳名字空间的浏览，很多较复杂的编程方法都没有表现出来。最后，希望本文能够起到抛砖引玉的作用，让更多的开发者认识与使用外壳名字空间，开发出更好的程序来。

## *参考资料* ##

- *[ Windows外壳名字空间的浏览](http://blog.csdn.net/kingcom_xu/article/details/18943 " Windows外壳名字空间的浏览 "). [2003-03-18].*
- MicrosoftCorporation. Microsoft Windows95程序员指南，清华大学出版社，1996
- StefamoMaruzzi.Windows95开发者必读，电子工业出版社，1997
