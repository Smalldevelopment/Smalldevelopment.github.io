---
title: iOS常用跳转
date: 2018-02-05 16:49:02
tags:
---
##### 打电话
1. 直接跳转拨号界面
```[[UIApplication sharedApplication] openURL:[NSURL URLWithString:@"tel://110"] options:@{} completionHandler:nil];```
2. 弹出拨号选择提示框(telprompt是私有方法上架可能会被禁止)
```[[UIApplication sharedApplication] openURL:[NSURL URLWithString:@"telprompt://110"] options:@{} completionHandler:nil];```
<!--more--> 
3. 创建一个UIWebView来加载URL，拨完后能自动跳回原来应用(这里的webView千万不要设置尺寸，不然会挡住其他页面)
```
if (_webView == nil) {
        _webView =[[UIWebView alloc] initWithFrame:CGRectZero];
    }
    [_webView loadRequest:[NSURLRequest requestWithURL:[NSURL URlWithString:@"tel//100"]]];
```
##### 发短信
1. 直接跳到发短信界面，但是不能指定短信内容，而且不能自动回到原来应用
```
[[UIApplication sharedApplication] openURL:[NSURL URLWithString:@"sms://110"] options:@{} completionHandler:nil];
```
2. 如果想指定短信内容可以使用MessageUI框架
```
导入头文件
#import <MessageUI/MessageUI.h>   
MFMessageComposeViewControllerDelegate
初始化代理
  MFMessageComposeViewController *messageVC = [[MFMessageComposeViewController alloc] init];     
  //发送短信内容 
  messageVC.body = @"你好？";
  //要发送的人
  messageVC.recipients = @[@"110",@"911"];
  //设置代理
  messageVC.messageComposeDelegate = self;
  [self presentViewController:messageVC animated:YES completion:nil];
```

```
// 当短信界面关闭的时候代用，发完后就会自动回到原应用
- (void)messageComposeViewController:(MFMessageComposeViewController *)controller didFinishWithResult:(MessageComposeResult)result
{
    //关闭短信界面
    [controller dismissViewControllerAnimated:YES completion:nil];
    if (result == MessageComposeResultCancelled) {
        
    }else if (result == MessageComposeResultSent)
    { 
    }else if (result == MessageComposeResultFailed)
    {
    }
}
```
##### 发邮件
1. 用苹果手机自带的邮件客户端，发送完邮件不会自动回到原应用
```
 [[UIApplication sharedApplication] openURL:[NSURL URLWithString:@"mailto://110@qq.com"] options:@{} completionHandler:nil];
```
2. 和上面发短信差不多
```
导入头文件
#import <MessageUI/MessageUI.h>   
MFMessageComposeViewControllerDelegate
//初始化代理
MFMailComposeViewController *mailVC = [[MFMailComposeViewController alloc] init];
//发送短信内容
[mailVC setMessageBody:@"你好?" isHTML:YES];
//要发送的人
[mailVC setToRecipients:@[@"",@""]];
//设置代理
mailVC.messageComposeDelegate = self;
[self presentViewController:mailVC animated:YES completion:nil];
```

```
- (void)mailComposeController:(MFMailComposeViewController *)controller didFinishWithResult:(MFMailComposeResult)result error:(NSError *)error
{
    关闭邮件界面
    [controller dismissViewControllerAnimated:YES completion:nil];
    
    if (result == MFMailComposeResultCancelled) {
        NSLog(@"取消发送");
    } else if (result == MFMailComposeResultSent) {
        NSLog(@"已经发出");
    } else {
        NSLog(@"发送失败");
    }
}
```
##### 跳转AppStore评分
```
//你应用在AppStore上面的ID
 NSString *appid =@"456464646";
 NSString *str = [NSString stringWithFormat: @"itms-apps://itunes.apple.com/cn/app/id%@?mt=8", appid];
 [[UIApplication sharedApplication] openURL:[NSURL URLWithString:str] options:@{} completionHandler:nil];
```


