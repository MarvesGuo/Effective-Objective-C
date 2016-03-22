# Effective-Objective-C     
a reading notes of 《Effective Objective－C 2.0》  &copy;Marves Guo


###1\. OC是C的超集，是动态时语言

####2\. 经量少的引入头文件

- @class向前引用能降低耦合度
- 避免循环引用（checken－and－age situation）
- 头文件分类清晰明了

###3\. 尽量多用字面量语法

- NSString       @“"
- NSNumber      @（expression）
    - @1
    - @2.5f
    - @3.141595653
    - @YES
    - @‘a’
- NSArray      @［］
    - array[［i］
    - 发现数组中nil对象，arrayWithObject无法识别
    - mutableArray［1］＝  @“Marves”，直接Replace
- NSDictionary  @｛｝
    - dict［@“Marves”］
    - mutableDict［@“a”］＝  @“Marves”，直接Replace

###4\. 少用＃define预处理，多用类型常量

- ＃define ANIMATION_DURATION 0.3
    - 引出的问题，多个文件都有的话，引用了会被替换掉
- static const NSTimeInterval kAnimationDuration = 0.3
    - extern NSString *const MKNotification；        （.h）
    - NSString *const MKNotification ＝ @“VALUE”；       （.m）

###5\.用枚举表示状态、选项、状态码  
    － typedef NS_OPTIONS(NSUInteger, UIViewAutoresizing) {  
      UIViewAutoresizingNone                 = 0,  
      UIViewAutoresizingFlexibleLeftMargin   = 1 << 0,  
      UIViewAutoresizingFlexibleWidth        = 1 << 1,  
      UIViewAutoresizingFlexibleRightMargin  = 1 << 2,  
      UIViewAutoresizingFlexibleTopMargin    = 1 << 3,  
      UIViewAutoresizingFlexibleHeight       = 1 << 4,  
      UIViewAutoresizingFlexibleBottomMargin = 1 << 5  
      };   
P17: 按位或运算  能保证一个数，多选且不重复  
    比如：  UIViewAutoresizingFlexibleBottomMargin｜UIViewAutoresizingFlexibleTopMargin  
    －switch语句和枚举结合不要用Default，多加的会有提示  

    //NS_OPTIONS  
    typedef NS_OPTIONS(NSUInteger, MyOption) {  
      MyOptionNone = 0, //二进制0000,十进制0  
      MyOption1 = 1 << 0,//0001,1  
      MyOption2 = 1 << 1,//0010,2  
      MyOption3 = 1 << 2,//0100,4  
      MyOption4 = 1 << 3,//1000,8  
      };  
     
    //声明定义枚举变量  
    MyOption option = MyOption1 | MyOption2;//0001 | 0010 = 0011,3  
       
    //检查是否包含某选型  
    if ( option & MyOption3 ){ //0011 & 0100 = 0000  
          //包含MyOption3  
    }else{  
         //不包含MyOption3  
    }  
     
    //增加选项:  
    option = option | MyOption4;//0011 | 1000 = 1011, 11  
    //减少选项  
    option = option & (~MyOption4);//1011 & (~1000) = 1011 & 0111 = 0011, 3  
