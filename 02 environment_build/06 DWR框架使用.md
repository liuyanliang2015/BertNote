## DWR是什么？
Dwr 是一个 Java 开源库，帮助你实现Ajax网站。它可以让你在浏览器中的Javascript代码调用Web服务器上的Java，就像在Java代码就在浏览器中一样。

Dwr 主要包括两部分：
在服务器上运行的 Servlet 来处理请求并把结果返回浏览器。
运行在浏览器上的 Javascript，可以发送请求，并动态改变页面。

Dwr 会根据你的 Java 类动态的生成Javascript代码。这些代码的魔力是让你感觉整个Ajax调用都是在浏览器上发生的，但事实上是服务器执行了这些代码，DWR负责数据的传递和转换。

## DWR执行原理

### 1、访问前端页面

http://localhost:8080/SpringMVC/led/toLed.do?userId=1

dwr会自动生成engine.js、util.js、interface/MessagePusher.js等虚拟文件。

执行JavaScript代码：MessagePusher.onPageLoad(userId);

就会调用java类MessagePusher的方法onPageLoad，同时触发ScriptSession管理器。

### 2、当Quartz定时任务，执行sendMessage代码

    MessagePusher push = new MessagePusher();
    //第一个参数0，表示广播
    push.sendMessage("0", "天气预报:"+RandomUtil.getRandomString(16));

 匹配的ScriptSession添加到Browser.getTargetSessions()中,待后面调用 
 
 异步任务：

    private ScriptBuffer script = new ScriptBuffer();
				public void run() {
					//对应页面调用的方法名称及参数
				    script.appendCall("showMessage", msg);
					Collection<ScriptSession> sessions = Browser.getTargetSessions();
					for (ScriptSession scriptSession : sessions) {
						scriptSession.addScript(script);
					}
				}


就会调用scriptSession对应页面的javscript方法showMessage()。

### 注意事项

1、第一次访问一个页面，就会创建一个新的HttpSession,之后再访问的时候，就会保持当前的Session,即使是刷新，也能保持当前的HttpSession。

2、ScriptSession不同，第一次访问，会创建一个ScriptSession,但是，如果你刷新，就会创建一个新的ScriptSession.



## DWR如何使用

### 1、引入dwr.jar

### 2、在web.xml集成dwr

    <!-- 配置DWR -->
	<servlet>
    <servlet-name>dwr-invoker</servlet-name>
    <servlet-class>org.directwebremoting.servlet.DwrServlet</servlet-class>
    <init-param>  
      <param-name >org.directwebremoting.extend.ScriptSessionManager </param-name>  
      <param-value >com.bert.dwr.DWRScriptSessionManager </param-value>  
	</init-param>  
    <init-param>
      <param-name>debug</param-name>
      <param-value>false</param-value>
    </init-param>
    <init-param>
      <param-name>activeReverseAjaxEnabled</param-name>
      <param-value>true</param-value>
    </init-param>
    <init-param>
      <param-name>initApplicationScopeCreatorsAtStartup</param-name>
      <param-value>true</param-value>
    </init-param>
    <init-param>
      <param-name>jsonRpcEnabled</param-name>
      <param-value>true</param-value>
    </init-param>
    <init-param>
      <param-name>jsonpEnabled</param-name>
      <param-value>true</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
	  </servlet>
	  <servlet-mapping>
	    <servlet-name>dwr-invoker</servlet-name>
	    <url-pattern>/dwr/*</url-pattern>
	  </servlet-mapping>


注意：
(1)dwr核心servlet处理器：org.directwebremoting.servlet.DwrServlet

(2)DWRScriptSessionManager 是scriptSession管理器

(3)url-pattern需要跟前端页面定义dwr需要js的路径，保持一致

例如：

    <!-- 路径从web.xml中配置,不需要引入真实的js，项目自动生成 -->
	<!-- 路径对应web.xml中配置的url-pattern -->
	<script type="text/javascript" src='<%=request.getContextPath()  %>/dwr/engine.js'></script>
	<script type="text/javascript" src='<%=request.getContextPath()  %>/dwr/util.js'></script>
	<script type="text/javascript" src='<%=request.getContextPath()  %>/dwr/interface/MessagePusher.js'></script>

### 3、引入dwr.xml

    <?xml version="1.0" encoding="UTF-8"?>
	<!DOCTYPE dwr PUBLIC "-//GetAhead Limited//DTD Direct Web Remoting 3.0//EN" "http://getahead.org/dwr/dwr30.dtd">
	
	<dwr>
	  <allow>
	    <!-- javascript对应前台引入的interface/MessagePusher.js文件名称 -->
	  	<create creator="new" javascript="MessagePusher"> 
	         <param name="class" value="com.bert.dwr.MessagePusher"/> 
	    </create> 
	  </allow>
	</dwr>

### 4、MessagePusher类

    package com.bert.dwr;

	import java.util.Collection;
	
	import org.directwebremoting.Browser;
	import org.directwebremoting.ScriptBuffer;
	import org.directwebremoting.ScriptSession;
	import org.directwebremoting.ScriptSessionFilter;
	import org.directwebremoting.WebContextFactory;
	import org.springframework.util.StringUtils;
	
	/**
	 * MessagePusher
	 */
	public class MessagePusher {
	public void onPageLoad(String userId) {
        if (!StringUtils.isEmpty(userId)){
        	ScriptSession scriptSession = WebContextFactory.get().getScriptSession();
        	scriptSession.setAttribute("userId", userId);
        	System.out.println("添加了一个scriptSession：  "+userId);
        }
	}
	
	public void sendMessage(final String userid, final String message) {
		try {
			final String id = userid;
	        final String msg = message;
			Browser.withAllSessionsFiltered(new ScriptSessionFilter() {
				//如果返回true,那将匹配的ScriptSession添加到Browser.getTargetSessions()中,待后面调用
				public boolean match(ScriptSession session) {
					String userId = (String) session.getAttribute("userId");
					if (userId == null) {
						return false;
					} else if ("0".equalsIgnoreCase(id)) {
						return true;
					} else {
						return userId.equalsIgnoreCase(id);
					}
				}
			}, new Runnable() {
				private ScriptBuffer script = new ScriptBuffer();
				public void run() {
					//对应页面调用的方法名称及参数
				    script.appendCall("showMessage", msg);
					Collection<ScriptSession> sessions = Browser.getTargetSessions();
					for (ScriptSession scriptSession : sessions) {
						scriptSession.addScript(script);
					}
				}
			});
		} catch (Exception e) {
			e.printStackTrace();
		}
		
	}
}


### 5、DWRScriptSessionManager and  DWRScriptSessionListener

    public class DWRScriptSessionManager extends DefaultScriptSessionManager {  
	  public DWRScriptSessionManager(){  
          //绑定一个ScriptSession增加销毁事件的监听器  
          this.addScriptSessionListener( new DWRScriptSessionListener());  
         System. out.println( "bind DWRScriptSessionListener");  
  	 }  
	}


    /**
	 * DWR ScriptSession 监听器
	 * DWRScriptSessionListener
	 */
	public class DWRScriptSessionListener implements ScriptSessionListener{
	//维护一个Map key为session的Id， value为ScriptSession对象  
    public static final Map<String, ScriptSession> scriptSessionMap = new HashMap<String, ScriptSession>();  
 
    /** 
     * ScriptSession创建事件 
     */  
    public void sessionCreated(ScriptSessionEvent event) {  
          WebContext webContext = WebContextFactory. get();  
          HttpSession session = webContext.getSession();  
          ScriptSession scriptSession = event.getSession();  
           scriptSessionMap.put(session.getId(), scriptSession);     //添加scriptSession  
          System. out.println( "session: " + session.getId() + " scriptSession: " + scriptSession.getId() + "is created!");  
    }  
 
    /** 
     * ScriptSession销毁事件 
     */  
    public void sessionDestroyed(ScriptSessionEvent event) {  
          WebContext webContext = WebContextFactory. get();  
          HttpSession session = webContext.getSession();  
          ScriptSession scriptSession = scriptSessionMap.remove(session.getId());  //移除scriptSession  
          System. out.println( "session: " + session.getId() + " scriptSession: " + scriptSession.getId() + "is destroyed!");  
    }  
      
    /** 
     * 获取所有ScriptSession 
     */  
    public static Collection<ScriptSession> getScriptSessions(){  
           return scriptSessionMap.values();  
    }  

}



### 6、前端调用

    <script type="text/javascript">
	var userId =  '${param.userId}';
	function init(){
		if (userId || userId != ''){
			dwr.engine.setActiveReverseAjax(true);
			dwr.engine.setNotifyServerOnPageUnload(true);
			MessagePusher.onPageLoad(userId);
		}
	}
	//页面加载时调用init函数
	window.onload = init;
	//对应appendCall设置的方法名称
	function showMessage(msg) {
		$("#msg").val(msg);
	}
	</script>


