# Tizen Demo App
Tizen Demo Web App that demonstrates ability to display integrated CMP

## Requirements
1. Tizen Studio

## Import project
If you already have Tizen Studio installed:
1. Clone the repo to your workspace directory
2. In Tizen Studio click File > Import menu item
3. Choose the directory where you have just cloned the repo
4. Select project to import from the list

## Running on the Samsung TV Simulator
To run the application on the simulator:
1. In the Project Explorer view, right-click the project and select Run As > Tizen Web Simulator Application (Samsung TV).

# Useful links
https://docs.tizen.org/application/web/get-started/overview/
https://developer.samsung.com/smarttv/develop/getting-started/using-sdk/tv-simulator.html

## Navigation class(navigation.js)
Available methods:
- `onLoad` runs on startup, trigger registerKey and bindEvents functions
- `getViewportWindow` get window object of an iframe
- `getActiveElement` get an active element of the document
- `triggerClick` handle enter button pressed event
- `registerKey` register a button of a remote control by key name
- `unregisterKey` unregister a button of a remote control by key name
- `onKeyDown` handle pressed keys of a remote control
- `bindEvents` add event listener from a remote control

## Implementation notes
To make the consent manager work just include a bundle of scripts into your index.html:
1. To display a Privacy Manager window include these scripts:
```
<script type="text/javascript">
    function _typeof(t){return(_typeof="function"==typeof Symbol&&"symbol"==typeof Symbol.iterator?function(t){return typeof t}:function(t){return t&&"function"==typeof Symbol&&t.constructor===Symbol&&t!==Symbol.prototype?"symbol":typeof t})(t)}!function(){for(var t,e,o=[],n=window,r=n;r;){try{if(r.frames.__tcfapiLocator){t=r;break}}catch(t){}if(r===n.top)break;r=n.parent}t||(function t(){var e=n.document,o=!!n.frames.__tcfapiLocator;if(!o)if(e.body){var r=e.createElement("iframe");r.style.cssText="display:none",r.name="__tcfapiLocator",e.body.appendChild(r)}else setTimeout(t,5);return!o}(),n.__tcfapi=function(){for(var t=arguments.length,n=new Array(t),r=0;r<t;r++)n[r]=arguments[r];if(!n.length)return o;"setGdprApplies"===n[0]?n.length>3&&2===parseInt(n[1],10)&&"boolean"==typeof n[3]&&(e=n[3],"function"==typeof n[2]&&n[2]("set",!0)):"ping"===n[0]?"function"==typeof n[2]&&n[2]({gdprApplies:e,cmpLoaded:!1,cmpStatus:"stub"}):o.push(n)},n.addEventListener("message",(function(t){var e="string"==typeof t.data,o={};if(e)try{o=JSON.parse(t.data)}catch(t){}else o=t.data;var n="object"===_typeof(o)?o.__tcfapiCall:null;n&&window.__tcfapi(n.command,n.version,(function(o,r){var a={__tcfapiReturn:{returnValue:o,success:r,callId:n.callId}};t&&t.source&&t.source.postMessage&&t.source.postMessage(e?JSON.stringify(a):a,"*")}),n.parameter)}),!1))}();
</script>
<script type="text/javascript">
  window._sp_queue = [];
  window._sp_ = {
      config: {
          accountId: 22,
          baseEndpoint: 'https://cdn.privacy-mgmt.com',
          propertyHref: 'http://tizen.app.com',
          gdpr: { }
      }
}
</script>
<script src="https://cdn.privacy-mgmt.com/unified/wrapperMessagingWithoutDetection.js"></script>
```

2. To get possibility navigate on the Privacy Manager include:
```
<script type="application/javascript">
function Navigation(e){e=e||{},this.viewportModal=e.viewport||"[id^='sp_message_iframe_']",this.tvKey={KEY_ENTER:13,KEY_0:48,KEY_RETURN:10009,KEY_EXIT:10182},this.onLoad()}Navigation.prototype={onLoad:function(){this.registerKey("0"),this.bindEvents()},getViewportWindow:function(){const e=document.querySelector(this.viewportModal);return!!e&&e.contentWindow},getActiveElement:function(e){e||(e=document.activeElement);const t=e.shadowRoot,n=e.contentDocument;return t&&t.activeElement?this.getActiveElement(t.activeElement):n&&n.activeElement?this.getActiveElement(n.activeElement):e},triggerClick:function(){const e=this.getActiveElement();var t=new Event("keydown",{bubbles:!0,cancelable:!0});t.keyCode=this.tvKey.KEY_ENTER,e.parentElement.className.split(" ").indexOf("categories")>=0&&(t=new KeyboardEvent("keypress",{key:"Enter",bubbles:!0,charCode:0,keyCode:this.tvKey.KEY_ENTER})),e.dispatchEvent(t)},registerKey:function(e){window.tizen.tvinputdevice.registerKey(e)},unregisterKey:function(e){window.tizen.tvinputdevice.unregisterKey(e)},onKeyDown:function(e){const t=tileNavigation.getViewportWindow();switch(e.keyCode){case tileNavigation.tvKey.KEY_ENTER:t&&tileNavigation.triggerClick();break;case tileNavigation.tvKey.KEY_EXIT:window.tizen.application.getCurrentApplication().exit();break;case tileNavigation.tvKey.KEY_0:window.location.reload();break;default:if(t){const n=new Event("keydown",{bubbles:!0,cancelable:!0});n.currentTarget=e.currentTarget,n.keyCode=e.keyCode,t.dispatchEvent(n)}}},bindEvents:function(){window.addEventListener("keydown",this.onKeyDown)}};
var tileNavigation,DemoCMP={};DemoCMP.LoadCMP=function(){tileNavigation=new Navigation;try{console.log("Widget version: "+window.tizen.application.getAppInfo().version)}catch(o){console.log(o)}},window.onload=DemoCMP.LoadCMP();</script>
```
## Global Privacy Platform (GPP) Multi-State Privacy (MSPS) Support

To enable the MSPS signal on your web property that is currently utilizing the U.S. Privacy String, your organization will need to add the GPP stub file in addition to the U.S. Privacy - [CCPA stub](https://docs.sourcepoint.com/hc/en-us/articles/4405412395667#h_01FDD1S943DFHN9NMATFMRVFNH) stub file to `index.html`: 

```
<script>
window.__gpp_addFrame=function(e){if(!window.frames[e])if(document.body){var t=document.createElement("iframe");t.style.cssText="display:none",t.name=e,document.body.appendChild(t)}else window.setTimeout(window.__gpp_addFrame,10,e)},window.__gpp_stub=function(){var e=arguments;if(__gpp.queue=__gpp.queue||[],__gpp.events=__gpp.events||[],!e.length||1==e.length&&"queue"==e[0])return __gpp.queue;if(1==e.length&&"events"==e[0])return __gpp.events;var t=e[0],p=e.length>1?e[1]:null,s=e.length>2?e[2]:null;if("ping"===t)p({gppVersion:"1.1",cmpStatus:"stub",cmpDisplayStatus:"hidden",signalStatus:"not ready",supportedAPIs:["2:tcfeuv2","5:tcfcav1","6:uspv1","7:usnatv1","8:uscav1","9:usvav1","10:uscov1","11:usutv1","12:usctv1"],cmpId:0,sectionList:[],applicableSections:[],gppString:"",parsedSections:{}},!0);else if("addEventListener"===t){"lastId"in __gpp||(__gpp.lastId=0),__gpp.lastId++;var n=__gpp.lastId;__gpp.events.push({id:n,callback:p,parameter:s}),p({eventName:"listenerRegistered",listenerId:n,data:!0,pingData:{gppVersion:"1.1",cmpStatus:"stub",cmpDisplayStatus:"hidden",signalStatus:"not ready",supportedAPIs:["2:tcfeuv2","5:tcfcav1","6:uspv1","7:usnatv1","8:uscav1","9:usvav1","10:uscov1","11:usutv1","12:usctv1"],cmpId:0,sectionList:[],applicableSections:[],gppString:"",parsedSections:{}}},!0)}else if("removeEventListener"===t){for(var a=!1,i=0;i<__gpp.events.length;i++)if(__gpp.events[i].id==s){__gpp.events.splice(i,1),a=!0;break}p({eventName:"listenerRemoved",listenerId:s,data:a,pingData:{gppVersion:"1.1",cmpStatus:"stub",cmpDisplayStatus:"hidden",signalStatus:"not ready",supportedAPIs:["2:tcfeuv2","5:tcfcav1","6:uspv1","7:usnatv1","8:uscav1","9:usvav1","10:uscov1","11:usutv1","12:usctv1"],cmpId:0,sectionList:[],applicableSections:[],gppString:"",parsedSections:{}}},!0)}else"hasSection"===t?p(!1,!0):"getSection"===t||"getField"===t?p(null,!0):__gpp.queue.push([].slice.apply(e))},window.__gpp_msghandler=function(e){var t="string"==typeof e.data;try{var p=t?JSON.parse(e.data):e.data}catch(e){p=null}if("object"==typeof p&&null!==p&&"__gppCall"in p){var s=p.__gppCall;window.__gpp(s.command,(function(p,n){var a={__gppReturn:{returnValue:p,success:n,callId:s.callId}};e.source.postMessage(t?JSON.stringify(a):a,"*")}),"parameter"in s?s.parameter:null,"version"in s?s.version:"1.1")}},"__gpp"in window&&"function"==typeof window.__gpp||(window.__gpp=window.__gpp_stub,window.addEventListener("message",window.__gpp_msghandler,!1),window.__gpp_addFrame("__gppLocator"));
</script>
```

Additionally, your organization will need to add the `includeGppApi` parameter to the `ccpa` object in your client configuration and set one of the following flag(s) depending on your organization's use case. [Click here](<https://github.com/SourcePointUSA/sp-roku-sdk/wiki/Global-Privacy-Platform-(GPP)-Multi%E2%80%90State-Privacy-(MSPS)>) for more information on each attribute, possible values, and examples for signatories and non-signatories of the MSPA.

If `includeGppApi` is set to `true`, the following MSPA arguments will be set accordingly:

- `MspaCoveredTransaction`: `"no"`
- `MspaOptOutOptionMode`: `"na"`
- `MspaServiceProviderMode`: `"na"`

Example:

```
window._sp_queue = [];
window._sp_ = {
    config: {
        accountId: 1584,
        baseEndpoint: 'https://cdn.privacy-mgmt.com',
        ccpa: { 
            includeGppApi: true
        },
        propertyHref: 'https://www.testdemo.com',
  
```

Optionally, your organization can customize support for the MSPS by configuring the MSPA attributes as part of the GPP config. 
```
window._sp_queue = [];
window._sp_ = {
    config: {
        accountId: 1584,
        baseEndpoint: 'https://cdn.privacy-mgmt.com',
        ccpa: { 
            includeGppApi: {
                "MspaCoveredTransaction": "yes",
                "MspaOptOutOptionMode": "yes",
                "MspaServiceProviderMode": "no"
            }
        },
        propertyHref: 'https://www.testdemo.com',
  
```
