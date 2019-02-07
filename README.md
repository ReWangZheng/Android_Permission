# Android权限机制详解
Android现在将所有权限分为两类，一类是*普通权限*，一类是*危险权限*。普通权限指的是**不会威胁到用户安全和隐私的权限，系统会自动帮我们授权**，危险权限指的是**可能会触及用户隐私，或者对设备安全性造成影响的权限**，这部分权限需要用户手动点击授权才行。

权限组名|权限名
:-:|:-:|
CALENDAR|READ_CALENDAR <br/> WRITE_CALENDAR
CAMERA|CAMERA
CONTACTS|READ_CONTACTS
LOCATION|ACCESS_FINE_LOCATION <br/> ACCESS_CRARSE_LOCATION
MICROPHONE|RECORD_AUDIO
PHONE|READ_PHONE_STATE<br/>CALL_PHONE<br/>READ_CALL_LOG<br/>WRITE_CALL_LOG<br/>ADD_VOICEMAIL<br/>USE_SIP<br/>PROCESS_OUTGOING_CALLS
SENSORS|BODY_SENSORS
SMS|SEND_SMS <br/> RECEIVE_SMS <br/> RECEIVE_WAP_PUSH <br/> RECEIVE_MMS
STORAGE|READE_EXTERNAL_STORAGE <br/> WRITE_EXTERNAL_STORAGE








# 在程序运行时申请权限    
&emsp;&emsp;如果我们在使用危险权限的时候，我们需要在运行时候让用户决定是否授予权限。
我们申请一个用户权限所用的方法是    
&emsp;&emsp;&emsp;&emsp;`ActivityCompat.requestPermissions(Content,String[],int)`    
&emsp;&emsp;这个方法有三个参数，第一个参数就是上下文没什么好说的，第二个参数就是一个字符串数组，这个数组包含了咱们要***查找的权限***，第三个参数就是***请求码***，这个请求码会反馈到如下方法里，我们可以通过判断请求码来进行响应操作，具体方式如下      
        
        onRequestPermissionsResult(int RequestCode,String[]permission,int[]grantRequests)
            {
            switch(requestCode){
                case 1:
                   if(grantResults.length>0 && grantResults[0]==PackageManager.PERMISSION_GRANTED)
                   {
                       Call();
                   }else{
                       Toast.makeText(this,"权限不足，无法运行",Toast.LENGTH_SHORT);
                   }
                    break;
                  }
            }

&emsp;&emsp;***grantRequests[]*** 里封装的是请求结果，然后与`PackageManager.PERMISSION_GRANTED`对比，这个值就是同意授权的意思,要注意啊跟权限有关的字段都被封装到***PackageManger***类里面的,可以通过静态字段直接访问。我们检查一个用户是否拥有某个权限的方法是

    ContextCompat.checkSelfPermission(Context,权限名),

如果有则返回`PackagerManager.PERMISSION_GRANTEND`
