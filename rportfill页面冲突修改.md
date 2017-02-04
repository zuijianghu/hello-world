# 报表上报以及报表审核当同一张报表同时打开不同操作项页面冲突，解决方案
大家在自己的页面用原先最外层的id加上一个随机数UniqueId构成新的id包一下，再在js中用新的id作为最外层的元素find一下
  - jsp:  
  <pre><code>  
    &lt;%@ page import="java.util.UUID" %>
    &lt;%  
    String UniqueId = UUID.randomUUID().toString();  
    %>
    &lt;div id="companyreport&lt;%=UniqueId%>">
        内容
    &lt;/div>
   </code></pre>  
   
  - js: 
   <pre><code>   
    var configMap = {
        uniqueId:''
    };
    var jqueryMap = {
        $blockTarget: null,
        $companyContext: null,
    };
    var setJqueryMap = function () {
        jqueryMap.$blockTarget = $('body');
        jqueryMap.$companyContext = jqueryMap.$blockTarget.find('#companyContext' + configMap.uniqueId);
    };  
    var demo = function () {  
        jqueryMap.$companyContext.find('#id').on('click', function(){  
          内容
          })  
    };
    init: function (uniqueId) {
            configMap.uniqueId = uniqueId;
             setJqueryMap();
          }
     </code></pre>
