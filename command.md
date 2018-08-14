## Commands & Usage

---

* #### Open

  Input:

  ```js
  conf:     configuration objects for init = {"AppName":"", "IOC":"", "DCenter":"", "AppKey":"", "UseWeb":""} 
   AppName: motebus MMA 
   IOC:     MMA of IOC
   DCenter: MMA of Device Center 
   AppKey:  app key 
   UseWeb:  can be 'websocket','ajax' or ''
   reg:     DC registration information (optional) 
   EiToken: device token 
   SToken:  app token 
  cb:       callback({ErrCode, ErrMsg, result})
  ```

  Example 1:

  ```js
  var conf = {"AppName":"", "IOC":"", "DCenter":"", "AppKey":"", "UseWeb":""} 
  conf.AppName = ‘myfunc’; 
  conf.DCenter = ‘dc@boss.ypcloud.com:6780’; 
  conf.AppKey = ‘YfgEeop5’;
  var mChat = require('motechat');
  mChat.Open(conf, function(result){
      console.log(‘init result=%s’, JSON.stringify(result));
  });
  ```

  Example 2: reg to DC directly

  ```js
  var conf = {"AppName":"", "IOC":"", "DCenter":"", "AppKey":"", "UseWeb":""}
  conf.AppName = ‘myfunc’;
  conf.DCenter = ‘dc@boss.ypcloud.com:6780’; 
  conf.AppKey = ‘YfgEeop5’;
  var reginfo = {"EiToken":"8dilCCKj","SToken":"baTi52uE"}; 
  var mChat = require('motechat'); 
  mChat.Open(conf, reginfo, function(result){
     console.log(‘init result=%s’, JSON.stringify(result));  
  });
  ```

#### 

* #### Close

  Input:

  ```js
  cb: callback({ErrCode, ErrMsg})
  ```

#### 

* #### Publish

  Input:

  ```js
  app:     name of the function 
  func:    user function entry which is published 
  cb:      callback({ErrCode, ErrMsg})
  ```

  Example:

  ```js
  var app = ‘motechat’;
  var XrpcMcService = {
       "echo": function(head, body){
           console.log("xrpc echo: head=%s", JSON.stringify(head));
           if (typeof body == 'object')
                 sbody = JSON.stringify(body);         
           else             
                 sbody = body;
           console.log("xrpc echo: body=%s", sbody);
           return {"echo":body};     
        }
  }
  mChat.Publish(app, XrpcMcService, function(result){
   console.log('motechat publish: result=%s', JSON.stringify(result));
  });
  ```

#### 

* #### Isolated

  Input:

  ```js
  func: user function entry which is published
  cb:   callback({ErrCode, ErrMsg})
  ```

  Example:

  ```js
  var XrpcMcSecService = { 
       "echo": function(head, body){
           console.log("xrpc echo: head=%s", JSON.stringify(head));
           if (type of body == 'object')
                  sbody = JSON.stringify(body);
           else             
                  sbody = body; 
           console.log("xrpc echo: body=%s", sbody);         
           return {"echo":body};
        }
  }
  mChat.Isolated(XrpcMcSecService, function(result){
   console.log('motechat isolated: result=%s', JSON.stringify(result));
  });
  ```

#### 

* #### Reg

  Input:

  ```js
  data: registration information = {"EiToken":"","SToken":"","WIP":""} 
   EiToken: device token 
   SToken: app token 
   WIP: WAN ip ( empty means the same as dc )
  cb: callback({ErrCode, ErrMsg, result})
  ```

  Example:

  ```js
  var mydev = {"EiToken":"8dilCCKj","SToken":"baTi52uE","WIP":""};
  mChat.Reg(mydev, function(result){
   console.log('StartSession result=%s', JSON.stringify(result));
  });
  ```

  Note: First time device registration, the _EiToken_ and _SToken_ fields are empty

* #### UnReg

  Input:

  ```js
  data: registration information = {"SToken":""} 
   SToken: app token
  cb: callback({ErrCode, ErrMsg} )
  ```

  Example:

  ```js
  var mydev = {"SToken":"baTi52uE"}; 
  mChat.UnReg(mydev, function(result){ 
   console.log('EndSession result=%s', JSON.stringify(result));
  });
  ```

#### 

* #### Call

  Input:

  ```js
  xrpc: xrpc control object, {"SToken":"", "Target":"", "Func":"", "Data":{},"SendTimeout": 6,"WaitReply": 12} 
   SToken: app token 
   Target: name of the target device 
   Func: name of the function on the target device
   Data: data object for function 
   SendTimeout: Integer, send message timeout in sec
   WaitReply: Integer, reply wait time in sec
  cb: callback({ErrCode, ErrMsg}) or callback(reply)
  ```

  Example:

  ```js
  var target = ‘myEi’;
  var func = ‘echo’;
  var data = {"time":"2018/4/24 10:12:08"};
  var t1 = 6;
  var t2 = 12;
  var xrpc = {"SToken":mydev.SToken,"Target":target,"Func":func,"Data":data, "SendTimeout":t1, "WaitReply":t2};
  mChat.Call( xrpc, function(reply){
   console.log('CallSession reply=%s', JSON.stringify(reply));
  });
  ```

#### 

* #### Send

  Input:

  ```js
  xmsg: xmsg control object, {"SToken":"", "From":"", "Target":"","Data":{}, "SendTimeout": 6,"WaitReply": 12}
   SToken: app token 
   From: DDN of source device 
   Target: can be DDN, EiName, EiType or EiTag of destination device
   Data: data which needs to be sent 
   SendTimeout: Integer, send message Timeout in sec
   WaitReply: Integer, reply Wait time in sec

  cb: callback({ErrCode,ErrMsg}) or callback(reply)
  ```

  Example:

  ```js
  var target = ‘myEi’;
  var data = {"message":"Hello World"}; 
  var ddn = GetSocketAttr('ddn', socket.id); 
  var stoken = GetSocketAttr('stoken', socket.id);
  var t1 = 6; 
  var t2 = 12; 
  var xmsgctl = {"SToken":stoken,"From":ddn,"Target":target,"Data":data, "SendTimeout":t1,"WaitReply":t2}; 
  mChat.Send(xmsgctl, function(reply){ 
   console.log('sendxmsg reply=%s', JSON.stringify(reply));
  });
  ```

#### 

* #### Get

  Input:

  ```js
  data: input data object, {"SToken":""} 
   SToken: app token
   mChat.Get(data, function(result){ 
   console.log(‘GetDeviceInfo result=%s’, result);
   });
  cb: callback({ErrCode, ErrMsg}) or callback(result)
  ```

  Example:

  ```js
  var data = {"SToken":mydev.SToken}; 
  mChat.Get(data, function(result){ 
   console.log(‘GetDeviceInfo result=%s’, result);
   });
  ```

#### 

* #### Set

  Input:

  ```js
  data: input data object, {"SToken":"", "EdgeInfo":{}} 
   SToken: app token
   EdgeInfo: {"EiName":"","EiType":"","EiTag":"","EiLoc":""} 
   mChat.Set(data, function(result){ 
   console.log(‘SetDeviceInfo result=%s’, result);
   });
  cb: callback({ErrCode, ErrMsg} ) or callback(result)
  ```

  Example:

  ```js
  var info = {"EiName":"myEi","EiType":".ei","EiTag":"#my","EiLoc":""};
  var data = {"SToken":mydev.SToken,"EdgeInfo":info}; 
  mChat.Set(data, function(result){ 
   console.log(‘SetDeviceInfo result=%s’, result);
   });
  ```

#### 

* #### Search

  Input:

  ```js
  data: input data object, {"SToken":"","Keyword":""}
   SToken:  app token 
   Keyword: search keyword
  cb: callback({ErrCode, ErrMsg} ) or callback(reply)
  ```

  Example:

  ```js
  var data = {"SToken":mydev.SToken, "Keyword":"#test"};
  mChat.Search(data, function(result){ 
   console.log(‘Search result=%s’, result);
   });
  ```

#### 

* #### OnEvent

  Input:

  ```js
  stype: "message" is for getxmsg, "state" is for state changed 
  cb: the user routine entry
  ```

  Output:  
  returned state is boolean \( true or false \) in nature

  Example:

  ```js
  var InmsgRcve = function(ch, head, from, to, msgtype, data){
    console.log('InmsgRcve: channel=%s, from=%s, to=%s, msgtype=%s, data=%s', ch, JSON.stringify(from), to, msgtype, JSON.stringify(data));
  }
  var InState = function(state){ 
   console.log(‘InState=%s’, state);
  }
  mChat.OnEvent('message',InmsgRcve);
  mChat.OnEvent('state', InState);
  ```



