# GWT-Liferay-Integration FR
Tutorial "how to integrate a GWT app in Liferay Portlet using a Div-Container"

TUTO !!!! Construire une portlet Liferay GoogleWebToolkit !!!!
(En utilisant un Div-Container)

1) En ligne de commande dans le fichier :

~/liferay-plugin-sdk-6.2/portlets/> ant -Dportlet.name=MonPortlet -Dportlet.display.name=”MonPortlet GWT” create

---> Bravo ta portlet est disponible dans le dernier dossier sous le nom de “MonPortlet-portlet”.
	
2) Ouvrir le fichier view.jsp présent dans ../portlets/MonPortlet-portlet/docroot et coller :

<script src="<%=request.getContextPath()%>/html/monapp.nocache.js"></script><div id="uniqueID"></div>

3) Dans MonApp/../client/MonApp.java (ne pas oublier d’utiliser une classe pour l’encapsulage de toute l’application) 
/!\ On ne peut afficher qu’un seul élément du RootPanel dans une portlet, bien vérifier en amont que l’on récupère dans un Panel toute l’ui de l’application.

public class MonApp implements EntryPoint {
	…
	public void onModuleLoad(){
…
RootPanel.get(“uniqueID”).add(MonAppUI.getPanel());
...
}
...
}

4) Compilation du projet (avec eclipse -> GWT Compile / avec ant en ligne de commande : ant build) 

5) Dans ../liferay-plugin-sdk-6.2/portlets/MonPortlet-portlet/docroot -> créer un dossier html
~/portlets/MonPortlet-portlet/docroot> mkdir html

6) Copier tout les fichiers (de compilation) du dossier 

../MonApp/war/monapp/ -> ../liferay-plugin-sdk-6.2/portlets/MonPortlet-portlet/docroot/html/ 

7) Copier les fichiers (sources de classes) du dossier

../MonApp/war/WEB-INF/classes -> ../liferay-plugin-sdk-6.2/portlets/MonPortlet-portlet/docroot/html/WEB-INF

8) Copier les fichiers (Fichier RPC dans le cas de l’utiliation d’une servlet RPC) du dossier

../MonApp/war/WEB-INF/deploy -> ../liferay-plugin-sdk-6.2/portlets/MonPortlet-portlet/docroot/html/WEB-INF

9) Copier le web.xml de l’appli gwt dans ../liferay-plugin-sdk-6.2/portlets/MonPortlet-portlet/docroot/WEB-INF/
Dans le cas de l’utilisation de servlets, il faut utiliser le PortalDeleguateServlet inclus dans liferay en éditant le fichier web.xml de la sorte : 

<servlet>
     <servlet-name>monService</servlet-name>
     <servlet-class>com.app.MonApp.server.MonServlet</servlet-class>
     <servlet-class>com.liferay.portal.kernel.servlet.PortalDelegateServlet</servlet-class>
     <init-param>
   	<param-name>servlet-class</param-name>
		<param-value>com.app.MonApp.server.MonServlet</param-value>
	</init-param>
 	 <init-param>
   	<param-name>sub-context</param-name>
    	<param-value>monService</param-value>
 	</init-param>
 	<load-on-startup>1</load-on-startup>
 </servlet>

 <servlet-mapping>
   <servlet-name>MonServlet</servlet-name>
   <url-pattern>/monapp/monservice</url-pattern>
 </servlet-mapping>

10) Il ne reste plus qu’a déployer l’application en ce déplaçant à la racine du portlet et en tapant « ant deploy » dans le terminal. Voilà votre application est déployée et le fichier .war généré est importable dans votre portail Liferay.
