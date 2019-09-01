title: 我的文档
---

## loading
```
    <div id="loading">
        <div class="loading-center-absolute">
            <div class="object object_four"></div>
            <div class="object object_three"></div>
            <div class="object object_two"></div>
            <div class="objectobject_one"></div>
        </div>
    </div>
    <style>
        #loading { left: 0; top: 0; height: 100%; width: 100%; position: absolute; z-index: 10; background-color: rgba(255, 255, 255, 0.62); border-radius: 6px;}
        #loading .loading-center-absolute { position: absolute; left: 50%; top: 20%; height: 200px; width: 200px; margin-top: -100px; margin-left: -100px; -ms-transform: rotate(-135deg);  -webkit-transform: rotate(-135deg);  transform: rotate(-135deg); }
        #loading .loading-center-absolute .object{ -moz-border-radius: 50% 50% 50% 50%; -webkit-border-radius: 50% 50% 50% 50%; border-radius: 50% 50% 50% 50%; position: absolute; border-top: 5px solid #1E6185; border-bottom: 5px solid transparent; border-left:  5px solid #1E6185; border-right: 5px solid transparent; -webkit-animation: animate 2s infinite; animation: animate 2s infinite;	 }
        #loading .loading-center-absolute .object_one{ left: 75px; top: 75px; width: 50px; height: 50px; }							
        #loading .loading-center-absolute .object_two{ left: 65px; top: 65px; width: 70px; height: 70px; -webkit-animation-delay: 0.2s; animation-delay: 0.2s; }
        #loading .loading-center-absolute .object_three{ left: 55px; top: 55px; width: 90px; height: 90px; -webkit-animation-delay: 0.4s; animation-delay: 0.4s; }
        #loading .loading-center-absolute .object_four{ left: 45px; top: 45px; width: 110px; height: 110px; -webkit-animation-delay: 0.6s; animation-delay: 0.6s; }
        @-webkit-keyframes animate { 50% { -ms-transform: rotate(360deg) scale(0.8);  -webkit-transform: rotate(360deg) scale(0.8);  transform: rotate(360deg) scale(0.8);  } }
        @keyframes animate { 50% { -ms-transform: rotate(360deg) scale(0.8);  -webkit-transform: rotate(360deg) scale(0.8);  transform: rotate(360deg) scale(0.8);  } }
    </style>
```