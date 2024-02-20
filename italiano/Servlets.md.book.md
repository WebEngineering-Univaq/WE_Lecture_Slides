---
title: Java Servlets     
course: Web Engineering
organization: University of L'Aquila

author: Giuseppe Della Penna
---


<!----------------- BEGIN SLIDE 001 it -------------------------->

#  Java Servlets     


<!----------------- COLUMN 1 -------------------------->

> 001




    
*Concetti e Programmazione di Base*


Giuseppe Della Penna

Università degli Studi di L'Aquila    
giuseppe.dellapenna@univaq.it    
http://people.disim.univaq.it/dellapenna


> *Questo documento si basa sulle slide del corso di Web Engineering, riorganizzate per una migliore esperienza di lettura. Non è un libro di testo completo o un manuale tecnico, e deve essere utilizzato insieme a tutti gli altri materiali didattici del corso. Si prega di segnalare eventuali errori o omissioni all'autore.*

> Quest'opera è rilasciata con licenza CC BY-NC-SA 4.0. Per visualizzare una copia di questa licenza, visitate il sito https://creativecommons.org/licenses/by-nc-sa/4.0

<!----------------- BEGIN TOC -------------------------->

 - [1. Fondamenti delle Servlet Java](#1-fondamenti-delle-servlet-java)

    - [1.1. Introduzione alle Servlet](#11-introduzione-alle-servlet)

    - [1.2. Dove e Come Eseguire una Servlet](#12-dove-e-come-eseguire-una-servlet)

    - [1.3. Contesto di una Web Application](#13-contesto-di-una-web-application)

    - [1.4. Struttura delle directory di una Web Application](#14-struttura-delle-directory-di-una-web-application)

    - [1.5. Inserimento di una servlet in una Web Application](#15-inserimento-di-una-servlet-in-una-web-application)

    - [1.6. Le Classi Base per le Servlet](#16-le-classi-base-per-le-servlet)

    - [1.7. Il Ciclo di Vita di una Servlet](#17-il-ciclo-di-vita-di-una-servlet)

    - [1.8. Scrivere una Classe Servlet](#18-scrivere-una-classe-servlet)

    - [1.9. Fornire Informazioni sulla Servlet](#19-fornire-informazioni-sulla-servlet)

    - [1.10. Inizializzare e Finalizzare la Servlet](#110-inizializzare-e-finalizzare-la-servlet)

    - [1.11. Scrivere la Risposta](#111-scrivere-la-risposta)

    - [1.12. Leggere la Richiesta](#112-leggere-la-richiesta)

 - [2. Le Sessioni](#2-le-sessioni)

    - [2.1. Introduzione alle Sessioni](#21-introduzione-alle-sessioni)

    - [2.2. Gestire le Sessioni con i Cookie](#22-gestire-le-sessioni-con-i-cookie)

    - [2.3. Gestire le Sessioni con le URL](#23-gestire-le-sessioni-con-le-url)

 - [3. Database nelle Web Application](#3-database-nelle-web-application)

    - [3.1. Java e i DBMS: il JDBC](#31-java-e-i-dbms-il-jdbc)

    - [3.2. Il Connection Pooling](#32-il-connection-pooling)

 - [4. Argomenti Avanzati](#4-argomenti-avanzati)

    - [4.1. ServletContextListener](#41-servletcontextlistener)

    - [4.2. Filter](#42-filter)

 - [5. Riferimenti](#5-riferimenti)

 - [6. Esempi di codice](#6-esempi-di-codice)



<!------------------- END TOC --------------------------> 

<!------------------- END SLIDE 001 it -------------------------->

<!----------------- BEGIN SLIDE 001b it -------------------------->

## 1. Fondamenti delle Servlet Java

<!------------------- END SLIDE 001b it -------------------------->

<!----------------- BEGIN SLIDE 002 it -------------------------->

### 1.1. Introduzione alle Servlet


<!----------------- COLUMN 1 -------------------------->

> 002




Le servlet sono particolari classi Java che vengono eseguite in server web opportunamente predisposti (**servlet containers**). 

Le servlet sono esposte all'esterno come risorse web standard (hanno, cioè, una URL che le caratterizza).

Le servlet agiscono nella tradizionale modalità *request/response*, tipica del *server side scripting*: quando l'utente ne fa richiesta tramite la URL associata, il server attiva la servlet, la esegue, e ne restituisce il risultato come contenuto della risorsa.

Le **servlet API** di Java permettono di programmare in maniera completamente indipendente dalle caratteristiche del server, del client e del protocollo di trasferimento.

Il codice è normale codice Java, che può far uso di tutte le librerie e le utilità del linguaggio, tra cui la connessione a tutti i DBMS tramite JDBC, l'uso di XML tramite JAXP, ecc.

Le servlet sostituiscono i classici CGI fornendo un elevatissimo grado di sicurezza, versatilità e astrazione al programmatore. 

<!------------------- END SLIDE 002 it -------------------------->

<!----------------- BEGIN SLIDE 003 it -------------------------->

### 1.2. Dove e Come Eseguire una Servlet


<!----------------- COLUMN 1 -------------------------->

> 003




Per eseguire una servlet è necessario disporre di un server che possa fungere da  **servlet container**, fornendo adeguato supporto alla loro attivazione ed esecuzione.

Il server *gratuito* più utilizzato a questo scopo è  **Tomcat** (Apache). 

Il tradizionale server Apache HTTPD non è invece adatto a contenere servlet, tuttavia se necessario è possibile integrarlo con una installazione di Tomcat tramite un opportuno *connector*.

Tomcat è un container  *leggero*, ed **implementa solo le tecnologie di base** per lo sviluppo delle web applications (*Servlet*, *JSP*). Il **JEE Web profile**  completo è disponibile in server più complessi, come *TomEE*, basato su Tomcat, o   *Glassfish*.

**Attenzione**: a partire dalla versione 10, Tomcat è stato aggiornato per supportare l'evoluzione della Java EE, cioè la Jakarta EE: le web application scritte precedentemente non potranno essere eseguite su Tomcat 10 a meno che non siano adattate, principalmente modificando i package delle classi javax.\* in jakarta.\* (vedasi https://tomcat.apache.org/migration-10.html) 

<!------------------- END SLIDE 003 it -------------------------->

<!----------------- BEGIN SLIDE 004 it -------------------------->

####  Configurazione di Apache Tomcat


<!----------------- COLUMN 1 -------------------------->

> 004



Apache Tomcat, disponibile per tutte le piattaforme (è esso stesso un programma Java) può essere scaricato dal sito http://tomcat.apache.org/.  

L'istallazione su Windows e Unix è semplificata dall'uso di script di installazione completamente automatici.

- Su entrambe le piattaforme, è possibile scegliere di avviare il server come **servizio** (windows) o   **demone** (unix), cioè in automatico, oppure   **manualmente**.

- È sempre necessario che l'istallazione di Java (o meglio del JRE) sia individuabile dallo script di istallazione di Tomcat. A questo scopo, verificare che la variabile di ambiente   **JAVA\_HOME** sia correttamente impostata.

Una volta mandato in esecuzione, Tomcat risponde di default alla porta   **8080**.

Connettendosi alla url http://localhost:8080/manager/ è possibile configurare il server tramite un'applicazione web e monitorare lo stato delle web application in esecuzione sul server.    

Per poter accedere all'amministrazione, è prima di tutto necessario **creare un utente con privilegi amministrativi**, aggiungendo al file **conf/tomcat-users.xml**  una riga come la seguente

```xml
<user username="admin" password="adminpass" roles="admin,manager"/>
``` 

<!------------------- END SLIDE 004 it -------------------------->

<!----------------- BEGIN SLIDE 005 it -------------------------->

### 1.3. Contesto di una Web Application


<!----------------- COLUMN 1 -------------------------->

> 005




Le applicazioni web sono eseguite in **contesti**. Ogni contesto, in generale, corrisponde a una particolare directory che viene configurata nel server e associata a una URL specifica. 

Ogni contesto ha associati un insieme di *attributi* definiti dal sistema e dall'utente (vedremo come tali attributi possono essere utilizzati, ad esempio, per configurare l'applicazione), accessibili tramite l'oggetto **ServletContext**, e può essere essere configurato per *eseguire del codice all'avvio dell'applicazione* (cioè quando l'applicazione web viene installata o il server su cui è stata installata viene avviato) *o arrestata* (cioè quando l'applicazione web viene disinstallata o il server in cui è stata installata viene arrestato) utilizzando gli oggetti **ServletcontextListener**.

Per creare manualmente un nuovo contesto è sufficiente creare una sottodirectory nella directory **webapps** di Tomcat. Il nome del contesto sarà quello della directory creata.

A questo punto, per testare il nuovo contesto, è possibile inserire un file html nella directory indicata e provare a caricarlo con la URL http://localhost:8080/PATH/NOMEFILE, dove path è il nome del contesto. Ad esempio http://localhost/progetto/index.html   

Tuttavia, per avere un'applicazione web totalmente funzionante, è anche  necessario approntare una particolare struttura di sottodirectory nel contesto e scrivere alcuni file di configurazione. I principali elementi di questa struttura verranno illustrati di seguito.

Tuttavia, **il metodo di installazione consigliato per le web application è quello di usare un IDE (ad esempio Netbeans) per realizzare l'applicazione ed effettuarne il packaging in un file *war* (Web ARchive), che potrà poi essere copiato *manualmente* nella directory webapps di Tomcat oppure installato *automaticamente* dall'IDE**. 

<!------------------- END SLIDE 005 it -------------------------->

<!----------------- BEGIN SLIDE 006 it -------------------------->

### 1.4. Struttura delle directory di una Web Application


<!----------------- COLUMN 1 -------------------------->

> 006




Le directory corrispondenti ad una web application hanno una struttura base particolare che permette al server di accedere alle risorse dinamiche (servlet, jsp) e statiche (html, css, immagini, ecc.). In particolare:        

- La sottodirectory **WEB-INF** contiene alcuni files di configurazione, tra cui il   *web application deployment descriptor*      (web.xml).

- La sottodirectory **WEB-INF/classes** contiene le classi Java dell'applicazione, comprese le servlet.  Secondo le convenzioni Java, le singole classi andranno poste all'interno di un albero di directory corrispondente al loro package. 

- La sottodirectory **WEB-INF/lib** contiene le librerie JAR necessarie all'applicazione, comprese quelle di terze parti, come i driver JDBC.  

- Tutti le altre sottodirectory del contesto, compresa la stessa root directory, conterranno normali files come pagine html, fogli di stile, immagini o pagine JSP. 

<!------------------- END SLIDE 006 it -------------------------->

<!----------------- BEGIN SLIDE 007 it -------------------------->

### 1.5. Inserimento di una servlet in una Web Application


<!----------------- COLUMN 1 -------------------------->

> 007



Un servlet è essenzialmente una classe Java che implementa l'interfaccia *Servlet*. Tuttavia, dopo aver compilato i relativi sorgenti e copiato il file di classe nella sottodirectory WEB-INF/classes, per renderlo disponibile come risorsa servlet, è necessario configurarne le funzionalità tramite un file denominato **web application deployment descriptor**. Questo file, denominato web.xml, deve essere inserito nella sottodirectory WEB-INF del contesto.

Un semplice esempio di descrittore è mostrato di seguito.

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>     
<!DOCTYPE web-app PUBLIC "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN" "http://java.sun.com/dtd/web-app_2_3.dtd">           
<web-app>  
 <display-name>Progetto IW</display-name>     
 <description>Progetto X</description>     
 <servlet>   
  <servlet-name>Servlet1</servlet-name>     
  <description>Questa servlet implementa la funzione Y </description>       
  <servlet-class>org.iw.project.class1</servlet-class>     
 </servlet>  
 <servlet-mapping>   
  <servlet-name>Servlet1</servlet-name>     
  <url-pattern>/funzione1</url-pattern>     
 </servlet-mapping>   
 <session-config>   
  <session-timeout>30</session-timeout>    
 </session-config>   
</web-app>   
```

 


<!----------------- COLUMN 2 -------------------------->



Ciascuna servlet è configurata in un elemento   **\<servlet\>**   distinto. L'elemento **\<servlet-class\>**   deve contenere il nome completo della classe che implementa la servlet.  

È necessario poi mappare ciascuna servlet su una URL tramite l'elemento   **\<servlet-mapping\>**. L'**\<url-pattern\>**   specificato andrà a comporre la URL per la servlet nel seguente modo:  

```
http://[server address]/[context]/[url pattern]      
``` 

<!------------------- END SLIDE 007 it -------------------------->

<!----------------- BEGIN SLIDE 008 it -------------------------->

### 1.6. Le Classi Base per le Servlet


<!----------------- COLUMN 1 -------------------------->

> 008




Alla base dell'implementazione di una servlet c'è l'interfaccia **Servlet**, che è implementata da una serie di classi base come **HttpServlet**. Tutte le servlet che creeremo saranno derivate (*extends*) da quest'ultima.

Le altre due classi base per la creazione di una servlet sono **ServletRequest** e **ServletResponse**.

Un'istanza di **ServletRequest** viene passata dal contesto alla servlet quando questa viene invocata, e contiene tutte le informazioni inerenti la richiesta effettuata dal client: queste comprendono, ad esempio, i parametri GET e POST inviati dal client, le variabili di ambiente del server, gli *header* e il *payload* della richiesta HTTP.

Un'istanza di **ServletResponse** viene passata alla servlet quando le si richiede di restituire del contenuto da inviare al client. I metodi di questa classe permettono di scrivere su uno *stream* che verrà poi indirizzato al client, modificare gli *header* della risposta HTTP, ecc. 

<!------------------- END SLIDE 008 it -------------------------->

<!----------------- BEGIN SLIDE 009 it -------------------------->

### 1.7. Il Ciclo di Vita di una Servlet


<!----------------- COLUMN 1 -------------------------->

> 009




Il ciclo di vita di una servlet è scandito da una serie di chiamate, effettuate dal container, a particolari metodi dell'interfaccia Servlet.    

- **Inizializzazione**. Quando il container carica la servlet, chiama il suo metodo   `init`. Tipicamente questo metodo viene usato per stabilire connessioni a database e preparare il contesto per le richieste successive. A seconda dell'impostazione del contesto e/o del contenitore, la servlet può essere caricata immediatamente all'avvio del server, ad ogni richiesta, ecc.  

- **Servizio**. Le richieste dei client vengono gestite dal container tramite chiamate al metodo `service`. Richieste concorrenti corrispondono a esecuzioni di questo metodo in thread distinti. L'implementazione dovrebbe essere quindi thread-safe. Il metodo service riceve le richieste dell'utente sotto forma di una ServletRequest e invia la risposta tramite una ServletResponse.        

- **Finalizzazione**. Quando il container decide di rimuovere la servlet, chiama il suo metodo   `destroy`. Quest'ultimo viene solitamente utilizzato per chiudere connessioni a database, o scaricare altre risorse persistenti attivate dal metodo `init`.  

La classe **HttpServlet** specializza questo sistema per la comunicazione HTTP, fornendo in particolare due metodi: `doGet` e `doPost`, corrispondenti alle due request più comuni in HTTP. Il metodo  `service` delle classe HTTPServlet provvede automaticamente a smistare le richieste al metodo opportuno. 

<!------------------- END SLIDE 009 it -------------------------->

<!----------------- BEGIN SLIDE 010 it -------------------------->

### 1.8. Scrivere una Classe Servlet


<!----------------- COLUMN 1 -------------------------->

> 010




Per scrivere una semplice servlet è necessario creare una classe che estende   **javax.servlet.http.HttpServlet**.

La logica di funzionamento della servlet va codificata nei metodi corrispondenti al verbo HTTP cui devono rispondere.  

- `doGet` viene chiamata in risposta a richieste GET e HEAD

- `doPost` viene chiamata in risposta a richieste POST

- `doPut` viene chiamata in risposta a richieste PUT

- `doDelete` viene chiamata in risposta a richieste DELETE

Tutti questi metodi hanno la stessa *signature*: prendono cioè come argomenti la coppia *(HttpServletRequest, HttpServletResponse)*     e restituiscono *void*.

Le classi  **HttpServletRequest** e **HttpServletResponse** sono specializzazioni di ServletRequest e ServletResponse specifiche per il protocollo HTTP.    

La classe HttpServlet fornisce un'implementazione di default di tutti i metodi appena descritti, che si limita a generare un errore 400 (BAD REQUEST)   

La classe Servlet contiene altri metodi utili, come  `getServletContext`, tramite il quale si possono leggere molte informazioni sul contesto di esecuzione della servlet stessa.  

Per compilare una servlet, è necessario che nel    **CLASSPATH** sia inclusa la libreria che definisce il package   **javax.servlet (o jakarta.servlet se si usa la Jakarta EE)**   . Una copia di questa libreria, chiamata  *servlet-api.jar*, è presente nella directory common/lib di Tomcat. 

<!------------------- END SLIDE 010 it -------------------------->

<!----------------- BEGIN SLIDE 011 it -------------------------->

####  Esempio


<!----------------- COLUMN 1 -------------------------->

> 011




```java
package org.iw.project;  

import javax.servlet.*;  
import javax.servlet.http.*;  

public class class1 extends HttpServlet {      
 public void doGet(HttpServletRequest in, HttpServletResponse out) {    
 //…
 }
}
```

 


<!----------------- COLUMN 2 -------------------------->



Una servlet di base estende la classe javax.servlet.http.HttpServlet   

Per gestire le richieste HTTP, si sovrascrivono opportunamente i metodi corrispondenti: in questo esempio, il metodo `doGet` verrà chiamato per gestire le richieste HTTP GET.  

> Si vedano le applicazioni di esempio: Java\_WebApp\_Base\_T10, Java\_WebApp\_Base_T9 

<!------------------- END SLIDE 011 it -------------------------->

<!----------------- BEGIN SLIDE 012 it -------------------------->

####  Esempio con annotazioni di deployment


<!----------------- COLUMN 1 -------------------------->

> 012




```java
package org.iw.project;  

import javax.servlet.*;  
import javax.servlet.http.*;  

@WebServlet(name = "Servlet1", urlPatterns = {"/funzione1"})        
public class class1 extends HttpServlet {      
 public void doGet(HttpServletRequest in, HttpServletResponse out) {    
 //…
 }
}
```

 


<!----------------- COLUMN 2 -------------------------->



È possibile trovare anche servlet annotate come in questo esempio.  

In questo caso, l'annotazione sostituisce i corrispondenti elementi *servlet* e *servlet-mapping* nel file web.xml, che possono quindi essere omessi (si veda l'esempio di deployment descriptor nelle slide precedenti). 

<!------------------- END SLIDE 012 it -------------------------->

<!----------------- BEGIN SLIDE 013 it -------------------------->

### 1.9. Fornire Informazioni sulla Servlet


<!----------------- COLUMN 1 -------------------------->

> 013




È possibile, anche se non richiesto, fornire delle informazioni sulla servlet che possono essere utilizzate dal container.

Le informazioni potrebbero ad esempio includere l'autore e la versione della servlet stessa.

A questo scopo è sufficiente sovrascrivere il metodo `getServletInfo()` dell'interfaccia Servlet, che nella sua implementazione di default in HttpServlet si limita a resitutire *null*.

Il metodo non prende alcun argomento e deve restituire una stringa. 

<!------------------- END SLIDE 013 it -------------------------->

<!----------------- BEGIN SLIDE 014 it -------------------------->

####  Esempio


<!----------------- COLUMN 1 -------------------------->

> 014




```java
package org.iw.project;  

import javax.servlet.*;  
import javax.servlet.http.*;  

public class class1 extends HttpServlet {      

 public String getServletInfo() {    
   return "Servlet di esempio, versione 1.0";    
 }

 public void doGet(HttpServletRequest in, HttpServletResponse out) {        
 //…
 }
}
```

 


<!----------------- COLUMN 2 -------------------------->



La stringa restituita da `getServletInfo` verrà usata dal container per fornire informazioni sulla servlet. 

<!------------------- END SLIDE 014 it -------------------------->

<!----------------- BEGIN SLIDE 015 it -------------------------->

### 1.10. Inizializzare e Finalizzare la Servlet


<!----------------- COLUMN 1 -------------------------->

> 015




L'inizializzazione di una servlet si effettua nel suo metodo `init`, che ha come parametro un oggetto *ServletConfig*.

La prima cosa che il metodo deve fare è chiamare `super.init()` passandogli il suo parametro ServletConfig.

Successivamente è possibile eseguire tutto il codice di inizializzazione necessario, eventualmente impostando campi della classe con dati che andranno utilizzati dai metodi di servizio.

Se la servlet è stata attivata con dei parametri di inizializzazione esterni (che vengono specificati in una maniera dipendente dal container), è possibile leggerli tramite il metodo `getInitParameter` di HttpServlet. Questo metodo prende come argomento il nome del parametro e restituisce una stringa.

Se l'inizializzazione incontra dei problemi, è possibile generare un'eccezione (*ServletException*) per segnalarlo al contanier.

La finalizzazione di una servlet si effettua nel suo metodo `destroy`.

È necessario sovrascrivere questo metodo solo se esistono effettivamente operazioni da fare prima della distruzione della servlet. 

<!------------------- END SLIDE 015 it -------------------------->

<!----------------- BEGIN SLIDE 016 it -------------------------->

####  Esempio


<!----------------- COLUMN 1 -------------------------->

> 016




```java
package org.iw.project;  

import javax.servlet.*;  
import javax.servlet.http.*;  
I
public class class1 extends HttpServlet {      
 private int parameter1;  

 public void init(ServletConfig c)      
  throws ServletException {    
  super.init(c);  
  parameter1 = 1;
 }

 public String getServletInfo() {    
   return "Servlet di esempio, versione 1.0";    
 }
 public void doGet(HttpServletRequest in, HttpServletResponse out) {        
 //…
 }
}
```

 


<!----------------- COLUMN 2 -------------------------->



Il metodo `init`, dopo aver chiamato lo stesso metodo della classe superiore, può procedere con l'inizializzazione della servlet.   

Se ci sono problemi di inizializzazione, viene generata una ServletException. 

<!------------------- END SLIDE 016 it -------------------------->

<!----------------- BEGIN SLIDE 017 it -------------------------->

### 1.11. Scrivere la Risposta


<!----------------- COLUMN 1 -------------------------->

> 017




L'oggetto **HttpServletResponse** fornito a tutti i metodi di servizio permette di costruire la risposta da inviare al client.

- Il metodo `setContentType(String)` permette di dichiarare il tipo restituito (ad esempio "text/xml").

- Il metodo `setHeader(String,String)` permette di aggiungere *headers* alla richiesta.

- Il metodo `sendRedirect(String)` permette di ridirezionare il browser verso una nuova URL.

- I metodi `getWriter()` e `getOutputStream()` permettono di aprire un canale, testuale o binario, su cui scrivere il contenuto della risposta. Vanno chiamati dopo aver (eventualmente) impostato il *content type* e gli altri *headers*.

Altri metodi (che non tratteremo in questa sede) permettono di gestire, ad esempio, i cookie. 

<!------------------- END SLIDE 017 it -------------------------->

<!----------------- BEGIN SLIDE 018 it -------------------------->

####  Esempio


<!----------------- COLUMN 1 -------------------------->

> 018




```java
package org.iw.project;  

import javax.servlet.*;  
import javax.servlet.http.*;  
import java.io.*;  
I
public class class1 extends HttpServlet {      
 //…
 public void doGet(HttpServletRequest in, HttpServletResponse out) {        
   out.setContentType("text/xml"); 
   try {  
     Writer w = out.getWriter();  
     w.write("pippo");    
   } catch(Exception e) {  
     e.printStackTrace();  
   }
 }
}
```

 


<!----------------- COLUMN 2 -------------------------->



Nel caso comune in cui si debba restituire dell'HTML al client, è sufficiente prelevare il   **Writer** della response tramite il metodo   `getWriter()`  e scrivervi sopra tutto il testo da restituire dal browser.

Qualunque altro parametro della risposta (in questo caso, il tipo) deve essere impostato *prima* di richiedere il canale di output.

> Si vedano le applicazioni di esempio: Java\_Example\_Servlet, Java\_Example\_Servlet\_Fwk, Java\_Example\_Downloader, Java\_Example\_Imager 

<!------------------- END SLIDE 018 it -------------------------->

<!----------------- BEGIN SLIDE 019 it -------------------------->

### 1.12. Leggere la Richiesta


<!----------------- COLUMN 1 -------------------------->

> 019




L'oggetto **HttpServletRequest** fornito a tutti i metodi di servizio contiene tutte le informazioni sulla richiesta del client.

- Il metodo `getParameter(String)`    restituisce il valore di un parametro inserito nella richiesta HTTP. Se il parametro può avere più di un valore, utilizzare `getParameterValues(String)`, che restituisce l'array di tutti i valori assegnati al parametro dato. 

- Per le richieste GET, il metodo  `getQueryString()`  permette di leggere l'intera stringa di query (non interpretata).

- Per le richieste POST, i metodi `getReader()`  e `getInputStream()`  permettono di aprire uno stream (testuale o binario) e leggere direttamente il payload della richiesta.    

- Il metodo `getHeader()`  permette di leggere il contenuto degli header della richiesta HTTP.  

- Il metodo `getSession()`  viene utilizzato per la gestione delle sessioni (si veda più avanti).

Altri metodi (che non tratteremo in questa sede) permettono di gestire autenticazione, cookie, ecc.

I metodi ereditati dalla classe **ServletRequest**, infine, permettono di leggere informazioni come l'indirizzo della richiesta (`getRemoteAddr()`) o il protocollo (`getProtocol()`).

**Questi metodi non sono direttamente validi nel caso in cui si utilizzi la codifica** ***multipart/form-data***  **(upload di files)!** 

<!------------------- END SLIDE 019 it -------------------------->

<!----------------- BEGIN SLIDE 020 it -------------------------->

####  Esempio


<!----------------- COLUMN 1 -------------------------->

> 020




```java
package org.iw.project;  

import javax.servlet.*;  
import javax.servlet.http.*;  
import java.io.*;  

public class class1 extends HttpServlet {      
 //…
 public void doGet(HttpServletRequest in, HttpServletResponse out) {        
   String p1 = in.getParameter("p1");    
 }
 public void doPost(HttpServletRequest in, HttpServletResponse out) {        
   try {  
     Reader r = in.getReader();  
     //lettura del payload raw da r…    
   } catch(Exception e) {  
     e.printStackTrace();  
   }
 }
}
```

 


<!----------------- COLUMN 2 -------------------------->



I metodi `doGet` e `doPost` possono leggere i parametri interpretati della richiesta tramite     `getParameter()`  e `getParameterValues()` .

- doGet può leggere la stringa di query in maniera raw chiamando      `getQueryString()`. 

- doPost può leggere il payload del messaggio in maniera raw tramite gli stream restituiti da        `getReader()`  (testuale) e `getInputStream()`  (binario). 

<!------------------- END SLIDE 020 it -------------------------->

<!----------------- BEGIN SLIDE 021 it -------------------------->

####  Multipart


<!----------------- COLUMN 1 -------------------------->

> 021




Nel caso in cui la richiesta arrivi al server in formato **multipart/form-data**    (tipicamente usata nel caso la richiesta contenga anche dei file binari) è necessario configurare la servlet che la dovrà ricevere in maniera opportuna, altrimenti sarà impossibile decodificarla.  

È quindi necessario inserire, all'interno del corrispondente elemento   *servlet* nel file web.xml (o come annotazione sulla classe servlet nel caso si utilizzi questa seconda modalità per dichiarare le servlet nel proprio progetto), la direttiva       **multipart-config**, tramite la quale è possibile anche specificare:

- La posizione in cui verranno temporaneamente scaricati i file allegati alla richiesta (**location**)

- La dimensione massima di ogni file allegabile alla richiesta (**max-file-size**)

- La dimensione massima complessiva della richiesta (**max-request-size**)

- La dimensione massima dei file che verranno conservati in memoria e non salvati sul disco (nella directory di cui sopra) (**file-size-threshold**)

Tutti i parametri di cui sopra hanno dei default ragionevoli, ma si consiglia sempre di configurare almeno le dimensioni massime dei file e delle richieste in base ai requisiti della propria applicazione, in modo da evitare anche possibili attacchi.

Dopo questa modifica, la servlet così configurata potrà accedere ai dati inviati usando la   `getParameter()`  già vista in precedenza, mentre per i file sarà necessario usare la `getPart()`  e poi lavorare con l'oggetto **Part** così ottenuto. 

<!------------------- END SLIDE 021 it -------------------------->

<!----------------- BEGIN SLIDE 022 it -------------------------->

####  Esempio multipart: configurazione servlet


<!----------------- COLUMN 1 -------------------------->

> 022




```xml
<web-app>   
 <display-name>Progetto IW</display-name>     
 <description>Progetto X</description>     
 <servlet>   
  <servlet-name>Servlet1</servlet-name>     
  <description>Questa servlet implementa la funzione Y </description>       
  <servlet-class>org.iw.project.class1</servlet-class>    
 <multipart-config>   
  <max-file-size>20848820</max-file-size>  
  <max-request-size>418018841</max-request-size> 
 </multipart-config>      
 </servlet>  
 <servlet-mapping>   
  <servlet-name>Servlet1</servlet-name>     
  <url-pattern>/funzione1</url-pattern>     
 </servlet-mapping>   
…
</web-app>   
```

 


<!----------------- COLUMN 2 -------------------------->



Per abilitare una specifica servlet a ricevere richieste codificate   **multipart**, è necessario aggiungere la direttiva *multipart-config* alla sua definizione.

Tramite la direttiva  **multipart-config** è possibile anche specificare: 

- La posizione in cui verranno temporaneamente scaricati i file allegati alla richiesta (**location**), default nella directory temp di sistema.  

- La dimensione massima di ogni file allegabile alla richiesta (**max-file-size**), default illimitata.

- La dimensione massima complessiva della richiesta (**max-request-size**), default illimitata.

- La dimensione massima dei file che verranno conservati in memoria e non salvati sul disco (nella directory di cui sopra) (**file-size-threshold**), default zero. 

<!------------------- END SLIDE 022 it -------------------------->

<!----------------- BEGIN SLIDE 023 it -------------------------->

####  Esempio multipart: codice servlet


<!----------------- COLUMN 1 -------------------------->

> 023




```java
public class class1 extends HttpServlet {      
 //…
public void doPost(HttpServletRequest in, HttpServletResponse out) {         

 try { 
  //ensure the request is multipart
  if (request.getContentType() != null && request.getContentType().startsWith("multipart/form-data")) {        
   //normal form field
   String p1 = request.getParameter("p1");      
   //uploaded file  
   Part f1 = request.getPart("f1");   
   if (f1 != null) {    
    String name = f1.getSubmittedFileName();       
    String contentType = f1.getContentType();      
    long fileSize = f1.getSize();    
    //move uploaded file from temp dir       
    //to web app repository (target)    
    Files.copy(f1.getInputStream(), target, StandardCopyOption.REPLACE_EXISTING);    
   }
  }
 } catch(Exception e) {  
  e.printStackTrace();  
 }
}
```

 


<!----------------- COLUMN 2 -------------------------->



In una servlet configurata per la ricezione di richieste   **multipart**, è possibile usare la normale `getParameter()` per tutti i dati trasmessi tranne che per i file.

Il metodo `getPart()` prende come argomento il nome del campo corrispondente al file e restituisce un oggetto **Part** tramite la quale è possibile leggere diverse informazioni sul file stesso come nome (`getSubmittedFilename()`), lunghezza (`getSize()`) e tipo (`getContentType()`).

Il file è temporaneamente scritto su disco e deve essere spostato in una directory gestita dall'applicazione nel caso sia necessario conservarlo.

A questo scopo è utile leggere il file sotto forma di stream ( `getInputStream()`).

> Si vedano le applicazioni di esempio: Java\_Example\_Servlet\_Multipart\_7, Java\_Example\_Uploader 

<!------------------- END SLIDE 023 it -------------------------->

<!----------------- BEGIN SLIDE 024 it -------------------------->

## 2. Le Sessioni


<!----------------- COLUMN 1 -------------------------->

> 024 

<!------------------- END SLIDE 024 it -------------------------->

<!----------------- BEGIN SLIDE 025 it -------------------------->

### 2.1. Introduzione alle Sessioni


<!----------------- COLUMN 1 -------------------------->

> 025



Il concetto di sessione è utilizzato ampiamente nella programmazione lato server. La sessione permette di **associare delle informazioni di stato alle richieste di un utente**, permettendo in pratica di aggirare la caratteristica *stateless* del protocollo HTTP.

Tipicamente, per gestire le sessioni si utilizza un **identificatore unico** (*session identifier* ) che viene assegnato all'utente al suo ingresso nell'applicazione web (ad esempio quando esegue la login) e trasmesso insieme ad ogni successiva richiesta http, per poi venire invalidato quando si effettua il logout o dopo un certo periodo di inattività.  

Il session identifier deve essere un numero ragionevolmente unico, e di solito viene generato con algoritmi random basati sulla data/ora corrente.  

Per trasmettere il session identifier con ogni richiesta, si utilizzano di solito due approcci:  

- **Cookies**: i cookie sono stringhe inviate dal server al browser. I browser ritrasmettono automaticamente al server i cookie di sua competenza insieme ad ogni request, quindi non è necessario alcun accorgimento lato client. I cosiddetti   *cookie di sessione* vengono cancellati dal browser alla chiusura, e contengono il session identifier.      
I cookie sono la soluzione più semplice e adottata, ma risentono delle politiche di sicurezza del browser (ad esempio la disattivazione dei cookies).

- **URL rewriting** : si tratta di inserire il session identifier all'interno delle URL, come parte del path o, più comunemente, come parametro GET.        
Si tratta della soluzione più generica e compatibile, che però richiede la generazione dinamica di tutte le pagine (ogni link deve essere generato inserendovi il session identifier corrente). 

<!------------------- END SLIDE 025 it -------------------------->

<!----------------- BEGIN SLIDE 026 it -------------------------->

### 2.2. Gestire le Sessioni con i Cookie


<!----------------- COLUMN 1 -------------------------->

> 026




Nelle servlet, creare ed utilizzare una sessione usando i cookies è molto semplice. La sessione è gestita da oggetti   **HttpSession**. Le *variabili di sessione* sono semplici stringhe a cui vengono associati generici valori di tipo Object.

Per prima cosa, si preleva un riferimento all'oggetto sessione richiedendolo alla **HttpServletRequest** tramite il metodo `getSession(boolean)` . 

Se il parametro di `getSession()` è true, una nuova sessione verrà creata nel caso non ce ne sia una valida già attiva. In caso contrario, la funzione potrebbe restituire null.    

La chiamata a questo metodo può modificare gli headers della risposta, per cui va eseguita prima di iniziare l'output della risposta stessa.   

I metodi di HttpSession permettono poi di utilizzare la sessione:  

- `isNew()`  restituisce true se la sessione è stata appena creata: in questo caso di solito vengono inizializzate le sue variabili di stato.  

- `getAttribute(String)`    restituisce l'oggetto associato al nome dato all'interno della sessione.

- `setAttribute(String, Object)`    associa al nome specificato l'oggetto passato come secondo argomento. In pratica, crea o aggiorna la variabile di stato data dal primo argomento usando il valore contenuto nel secondo argomento.

- `removeAttribute(String)`    rimuove la variabile di stato indicata.

- `invalidate()` chiude la sessione ed elimina tutte le informazioni di stato associate. 

<!------------------- END SLIDE 026 it -------------------------->

<!----------------- BEGIN SLIDE 027 it -------------------------->

####  Esempio 1


<!----------------- COLUMN 1 -------------------------->

> 027




```java
package org.iw.project;  

import javax.servlet.*;  
import javax.servlet.http.*;  
import java.io.*;  
I
public class class1 extends HttpServlet {      
 //…
 public void doGet(HttpServletRequest in, HttpServletResponse out) {        
   HttpSession s = in.getSession(true);      
   if (s.isNew()) s.setAttribute("pagine",new Integer(1));        
   int a = ((Integer)s.getAttribute("pagine")).intValue();        
   s.setAttribute("pagine", new Integer(a+1));     
   try {  
     Writer w = out.getWriter();  
     w.write("pagine visitate in questa sessione: "+a);  
   } catch(Exception e) {  
     e.printStackTrace();  
   }
 }
}
```

 


<!----------------- COLUMN 2 -------------------------->



Ad ogni richiesta GET alla servlet, se una sessione non è attiva, ne viene creata una e al suo interno viene inserita una variabile denominata "pagine" inizializzata a 1 (notare l'uso della classe Integer)    

Il numero di pagine visitato durante la sessione viene quindi incrementato e stampato in output.

> Si vedano le applicazioni di esempio: Java\_Example\_Servlet\_Sessions 

<!------------------- END SLIDE 027 it -------------------------->

<!----------------- BEGIN SLIDE 028 it -------------------------->

####  Esempio 2


<!----------------- COLUMN 1 -------------------------->

> 028




```java
package org.iw.project;  

import javax.servlet.*;  
import javax.servlet.http.*;  
import java.io.*;  

public class class1 extends HttpServlet {      
 //…
 public void doGet(HttpServletRequest in, HttpServletResponse out) {        
   HttpSession s = in.getSession(true);      
   s.setAttribute("user", in.getParameter("username"));    
 }
}
```

 


<!----------------- COLUMN 2 -------------------------->



Questa semplice servlet mostra come salvare all'interno della sessione il valore del parametro "username" prelevato dalla request (probabilmente proveniente da una form).      

Si tratta di una tipica operazione effettuata per concludere una procedura di login, tenendo traccia dell'utente attivato nella sessione corrente. 

> Si vedano le applicazioni di esempio: Java\_Example\_Login, Java\_Example\_Login\_Middleware 

<!------------------- END SLIDE 028 it -------------------------->

<!----------------- BEGIN SLIDE 029 it -------------------------->

### 2.3. Gestire le Sessioni con le URL


<!----------------- COLUMN 1 -------------------------->

> 029




Le servlet dispongono anche di un sistema semiautomatico per gestire le sessioni tramite la riscrittura delle URL.

In pratica, oltre al codice di gestione/creazione/uso della sessione visto precedentemente, è necessario far modificare ogni indirizzo internet che punta a una risorsa della nostra applicazione dal metodo `encodeUrl(String)` dell'oggetto **HttpServletResponse**. 

Il metodo encodeURL determina se è necessario inserire il session identifier nella URL: nel caso in cui il metodo dei cookie sia utilizzabile, la URL non viene alterata. 

<!------------------- END SLIDE 029 it -------------------------->

<!----------------- BEGIN SLIDE 030 it -------------------------->

####  Esempio


<!----------------- COLUMN 1 -------------------------->

> 030




```java
package org.iw.project;  

import javax.servlet.*;  
import javax.servlet.http.*;  
import java.io.*;  
I
public class class1 extends HttpServlet {      
 //…
 public void doGet(HttpServletRequest in, HttpServletResponse out) {        
   HttpSession s = in.getSession(true);      
   try {  
     Writer w = out.getWriter();  
     w.write(  out.encodeUrl(in.getServletPath())   );
   } catch(Exception e) {  
     e.printStackTrace();  
   }
 }
}
```

 


<!----------------- COLUMN 2 -------------------------->



In questo esempio, viene creata (se necessario) una sessione e sulla pagina viene stampata la URL della servlet corrente riscritta da  `encodeURL()` per includere il session identifier.  

*Se il browser supporta i cookies, la URL non verrà modificata*. 

<!------------------- END SLIDE 030 it -------------------------->

<!----------------- BEGIN SLIDE 031 it -------------------------->

## 3. Database nelle Web Application


<!----------------- COLUMN 1 -------------------------->

> 031 

<!------------------- END SLIDE 031 it -------------------------->

<!----------------- BEGIN SLIDE 032 it -------------------------->

### 3.1. Java e i DBMS: il JDBC


<!----------------- COLUMN 1 -------------------------->

> 032




Una delle operazioni più comuni in un'applicazione web è la **gestione di dati immagazzinati in un database**.

L'accesso ai dati in Java si effettua usando il package **JDBC** (*Java DataBase Connectivity*). Un uso tipico delle classi JDBC prevede i seguenti passi:

- Si rende disponibile il **driver JDBC** per il DBMS in uso nel classpath di Java.

- Si carica il driver facendo riferimento alla classe che lo implementa tramite metodo `Class.forName`

- Si procede con la creazione dell'oggetto **Connenction** tramite il metodo statico `getConnetion` della classe **DriverManager**.     
   I tre parametri del metodo sono **il nome utente e password** da usare per l'accesso al DBMS e la **stringa di connessione JDBC**, che specifica l'indirizzo del DBMS e il database da selezionare: Questa stringa ha un formato che varia a seconda del DBMS in uso.

- Si crea un oggetto **Statement** sulla connessione, usando il metodo `createStatement`.

- Si invia la query SQL, sotto forma di stringa, al DBMS tramite lo **Statement** creato e il suo metodo `executeQuery`.    
L'oggetto restituito, di tipo **ResultSet**, permette di navigare tra i risultati della query.     
Se invece si desidera inviare una query che non restituisce risultati, ad esempio di inserimento o aggiornamento, si usa il metodo `executeUpdate`. In questo caso, il valore restituito è un intero che rappresenta il numero di record interessati dalla query stessa.

- Una volta prelevati i risultati, si libera lo spazio a loro riservato chiamando il metodo `close` dello **Statement**.

- Infine, terminato l'uso della base di dati, si può chiudere la connessione corrispondente chiamando il metodo `close` della **Connection**.

Tutte le istruzioni JDBC, in caso di errore, sollevano eccezioni derivate da **SQLException**. 

<!------------------- END SLIDE 032 it -------------------------->

<!----------------- BEGIN SLIDE 033 it -------------------------->

####  Esempio


<!----------------- COLUMN 1 -------------------------->

> 033




```java
import java.sql.*;  

Class.forName ("com.mysql.cj.jdbc.Driver");   

Connection con = DriverManager.getConnection("jdbc:mysql://localhost/webdb?connectionTimeZone=LOCAL&amp;forceConnectionTimeZoneToSession=false","user", "pass");          

Statement stmt1 = con.createStatement();  
ResultSet rs = stmt1.executeQuery("SELECT * FROM test");
…
rs.close(); 
stmt1.close();

Statement stmt2 = con.createStatement();  
int rc = stmt.executeUpdate("DELETE FROM test");     
stmt2.close();

con.close(); 
```

 


<!----------------- COLUMN 2 -------------------------->



In questo esempio si crea una connessione a un database *MySQL*.

La classe del driver JDBC è   *com.mysql.cj.jdbc.Driver*.  Nota: se usate una versione del driver MySQL precedente alla 8, il nome della classe è c   *om.mysql.jdbc.Driver*. 

Attenzione: molti server hanno dei driver preinstallati per database comuni. Tuttavia potrebbero non averne l'ultima versione, soprattutto nel caso del driver MySQL versione 8. In questo caso, aggiungete il driver alla vostra applicazione!    

La stringa di connessione specifica il tipo di DBMS (*mysql*) il punto di ascolto del DBMS (*localhost*), e il database da selezionare (*webdb*). Può inoltre includere altri parametri specificati come una *query string* . 

Per il driver MySQl, dalla versione 8, è utile usare i parametri qui esposti per allineare la     *timezone* del JDBC con quella del server.

Alla connessione vengono anche passati la username e la password dell'utente con cui autenticarsi nel DBMS.

Viene eseguita prima una query di selezione tramite   `executeQuery` e poi una di cancellamento tramite `executeUpdate`. 

<!------------------- END SLIDE 033 it -------------------------->

<!----------------- BEGIN SLIDE 034 it -------------------------->

####  I ResultSet


<!----------------- COLUMN 1 -------------------------->

> 034




Tramite il **ResultSet** restituito dal metodo `executeQuery` è possibile leggere le colonne di ciascun record restituito da una query di selezione.

I record devono essere letti uno alla volta: in ogni momento, il **ResultSet** punta (tramite un *cursore*) a uno dei record restituiti (*record corrente*).

I valori dei vari campi del record corrente possono essere letti tramite i metodi `getX(nome_colonna)`, dove *X* è il **tipo Java** per il dato da estrarre (ad esempio `getString`, `getInt`, …) e *nome\_colonna* è il nome del campo del record da leggere.

Per spostare il cursore del **RecordSet** al record successivo, si usa il metodo `next`. Il metodo restituisce *false* quando i record sono esauriti. 

<!------------------- END SLIDE 034 it -------------------------->

<!----------------- BEGIN SLIDE 035 it -------------------------->

####  Esempio ResultSet


<!----------------- COLUMN 1 -------------------------->

> 035




```java
import java.sql.*;  

Class.forName ("com.mysql.cj.jdbc.Driver");   

Connection con = DriverManager.getConnection("jdbc:mysql://localhost/webdb?connectionTimeZone=LOCAL&amp;forceConnectionTimeZoneToSession=false","user", "pass");          

Statement stmt1 = con.createStatement();  

ResultSet rs = stmt1.executeQuery("SELECT * FROM test");

while (rs.next()) {   
 System.out.println("nome = "+ rs.getString("nome"));    
}

rs.close(); 
stmt1.close();
con.close(); 
```

 


<!----------------- COLUMN 2 -------------------------->



In questo esempio, utilizziamo un ciclo *while* per iterare tra i risultati di una query di selezione.  

Per ogni record viene stampato il valore del campo *nome*, di tipo stringa. 

<!------------------- END SLIDE 035 it -------------------------->

<!----------------- BEGIN SLIDE 036 it -------------------------->

####  Limiti del metodo d'uso standard


<!----------------- COLUMN 1 -------------------------->

> 036




In un'applicazione web *data intensive*, in cui gli accessi al database sono molti e spesso concorrenti (più utenti connessi all'applicazione contemporaneamente), il "normale" pattern d'uso de JDBC presenta notevoli problemi.

Infatti, **l'apertura di una connessione al database è di solito un'operazione costosa**, in quanto come si è visto richiede

- Caricamento driver

- Connessione al DBMS server

- Autenticazione

È necessario limitare l'overhead dovuto a queste operazioni, per rendere la web application più veloce possibile. 

<!------------------- END SLIDE 036 it -------------------------->

<!----------------- BEGIN SLIDE 037 it -------------------------->

### 3.2. Il Connection Pooling


<!----------------- COLUMN 1 -------------------------->

> 037




Il connection pooling è una tecnica che permette di   **snellire le procedure di apertura delle connessioni JDBC** utilizzando una **cache di connessioni**, chiamata *connection pool*.

Il pool mantiene una serie di connessioni al database **già aperte e inizializzate**.

Quando l'applicazione vuole connettersi, **preleva dal pool una connessione già pronta**, sulla quale può operare direttamente.

Quando l'applicazione chiude la connessione, **questa in realtà rimane aperta e viene reinserita nel pool**, in attesa di essere riutilizzata per altre richieste.

Il pool viene inizialmente riempito con un certo numero di connessioni pronte. Tuttavia, se il pool si svuota (cioè tutte le sue connessioni sono in uso) e c'è richiesta di nuove connessioni, queste vengono create automaticamente, allargando il pool.

Le connessioni lasciate inutilizzate nel pool per troppo tempo possono venir chiuse automaticamente per liberate le corrispondenti risorse del DBMS. 

<!------------------- END SLIDE 037 it -------------------------->

<!----------------- BEGIN SLIDE 038 it -------------------------->

####  Supporto negli application server


<!----------------- COLUMN 1 -------------------------->

> 038




Il supporto al connection pooling, incorporato nelle più recenti versioni del JDBC, deve ovviamente avere supporto da parte del software (proprio come con i driver JDBC).  

Tutti i più diffusi application server forniscono un'implementazione del connection pooling, ed esistono anche librerie esterne utilizzabili per aggiungere questo supporto ad ogni applicazione (non web).

Attenzione: **ogni application server fornisce sistemi proprietari per configurare il connection pooling**. Vedremo in particolare come si utilizza il connection pooling su Tomcat. 

<!------------------- END SLIDE 038 it -------------------------->

<!----------------- BEGIN SLIDE 039 it -------------------------->

####  Uso con Tomcat


<!----------------- COLUMN 1 -------------------------->

> 039




Per configurare il connection pooling in Tomcat, si procede come segue:    

- Si **configura la connessione al database nel contesto** della web application, agendo sul server.xml (globale) o, meglio, sul context.xml (configurazione specifica dell'applicazione).      

- Si inserisce nel deployment descriptor (web.xml) un riferimento alla connessione, che diventa così **una risorsa dell'applicazione di tipo DataSource** .

- Nel codice, si preleva un riferimento al DataSource utilizzando lo standard   *Java Naming and Directory Interface* (JNDI).

- Si crea una normale connessione JDBC attraverso il DataSource.              

- Si chiude la connessione al termine del suo uso. 

<!------------------- END SLIDE 039 it -------------------------->

<!----------------- BEGIN SLIDE 040 it -------------------------->

####  Configurazione del contesto dell'applicazione (context.xml) per Tomcat


<!----------------- COLUMN 1 -------------------------->

> 040




```xml
<Context path="/Esempio_Database_Pooling">      
 <Resource
  name="jdbc/webdb2"     
  type="javax.sql.DataSource"    
  auth="Container"  
  driverClassName="com.mysql.cj.jdbc.Driver"
  url="jdbc:mysql://localhost/webdb?connectionTimeZone=LOCAL&amp;forceConnectionTimeZoneToSession=false"             
  username="website"
  password="webpass"  
  maxActive="10"  
  maxIdle="5"  
  maxWait="10000"  
/>
</Context>  
```

 


<!----------------- COLUMN 2 -------------------------->



La connessione è configurata all'interno di un elemento **Resource**, che ne contiene tutte le caratteristiche, compreso il driver, la username, la password e la stringa di connessione.

Il *type* deve essere indicato come **javax.sql.DataSource**. L'attributo *auth* va impostato su "container".

Gli attributi *maxActive*, *maxIdle* e *maxWait* servono a dimensionare il pool, indicando

- Quante connessioni al massimo il pool dovrà contenere

- Quante connessioni inutilizzate sono ammesse nel pool in ogni momento

- Quanto tempo attendere perché una nuova connessione diventi disponibile

**Attenzione: il driver indicato andrà copiato nella directory lib di Tomcat. Se lo si copia semplicemente nella WEB-INF/lib dell'applicazione (come parte del deployment), esso sarà invisibile al class loader usato per il pooling!**              

**Attenzione: il nome della classe e la struttura della connection string possono cambiare in base alla versione del driver in uso da Tomcat (vedi esempi precedenti)** 

<!------------------- END SLIDE 040 it -------------------------->

<!----------------- BEGIN SLIDE 041 it -------------------------->

####  Configurazione del deployment descriptor (web.xml)


<!----------------- COLUMN 1 -------------------------->

> 041




```xml
<web-app version="2.5" xmlns="http://java.sun.com/xml/ns/javaee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd">                  
…
<resource-ref>  
  <res-ref-name>jdbc/webdb2</res-ref-name>          
  <res-type>javax.sql.DataSource</res-type>      
  <res-auth>Container</res-auth>    
  <res-sharing-scope>Shareable</res-sharing-scope>      
</resource-ref>  
…
</web-app>  
```

 


<!----------------- COLUMN 2 -------------------------->



Nel deployment descriptor si inserisce un riferimento alla risorsa JNDI con un elemento **resource-ref**.

Gli attributi *res-type*  e *res-auth*  rispecchiano quelli *type* e *res* scritti nella definizione della **Resource**

*res-sharing-scope*  dovrebbe generalmente essere posto a "Shareable"  

L'attributo *res-ref-name*    indica il nome JNDI utilizzato per la risorsa all'interno del codice. Le risorse di tipo DataSource hanno convenzionalmente un nome che inizia con "jdbc/". 

<!------------------- END SLIDE 041 it -------------------------->

<!----------------- BEGIN SLIDE 042 it -------------------------->

####  Uso nel codice Java


<!----------------- COLUMN 1 -------------------------->

> 042




```java
try { 
 //preleviamo un riferimento al naming context   
  InitialContext ctx = new InitialContext();      
  //e otteniamo un riferimento al DataSource 
 DataSource ds =  (DataSource) ctx.lookup("java:comp/env/jdbc/webdb2");          
  //connessione al database locale
  connection = ds.getConnection();  
 //…usiamo la connessione…
} catch (NamingException ex) {  
 //eccezione sollevata nel caso la risorsa richiesta non esista
} catch (SQLException ex) {  
 //eccezione standard per le operazione JDBC
} finally {  
 //alla fine le connessione DEVE essere chiusa!
  try { connection.close(); } catch (SQLException ex) {}      
}
```

 


<!----------------- COLUMN 2 -------------------------->



Si crea prima di tutto un *JNDI naming context* (**InitialContext**)

Si ricerca quindi nel contesto la risorsa necessaria. Notare che al nome configurato nel deployment descriptor va aggiunto sempre il prefisso "java:comp/env/". Può essere utile usare un parametro della web application per configurare questo nome al di fuori del codice.          

Si fa un cast dell'oggetto restituito sul tipo effettivo della risorsa (**DataSource**)

Infine si crea la **Connection** JDBC usando il metodo `getConnection()`  del DataSource.  

Dopo aver operato sulla connessione, la si chiude normalmente, determinandone il reinserimento nel pool. **Attenzione: se non si chiude, la connessione non rientrerà nel pool!** 

<!------------------- END SLIDE 042 it -------------------------->

<!----------------- BEGIN SLIDE 043 it -------------------------->

####  Resource Injection


<!----------------- COLUMN 1 -------------------------->

> 043




```java
class DatabaseService {   
 @Resource(name ="java:comp/env/jdbc/webdb2")        
 private DataSource ds;    
 //…
 public void dbMethod() {    
  try {  
   connection = ds.getConnection();  
   //…usiamo la connessione…
  } catch (SQLException ex) {  
   //eccezione standard per le operazione JDBC
  } finally {  
    //alla fine le connessione DEVE essere chiusa!
    try { connection.close(); } catch (SQLException ex) {}      
  }
 }
}
```

 


<!----------------- COLUMN 2 -------------------------->



La **Resource injection**   permette di evitare il complesso sistema di lookup delle risorse, e fare in modo che sia Java stesso a iniettare un riferimento al   *DataSource* in una variabile utente.

L'injection si effettua di solito su un campo della classe, che   **deve essere del tipo corretto** (*DataSource*).

Basta far precedere la dichiarazione del campo dall'annotazione `@Resource`, con parametro *name* uguale al nome della risorsa da prelevare.

> Si vedano le applicazioni di esempio: Java\_Example\_Servlet\_Database, Java\_Example\_Newspaper\_DAO 

<!------------------- END SLIDE 043 it -------------------------->

<!----------------- BEGIN SLIDE 044 it -------------------------->

## 4. Argomenti Avanzati


<!----------------- COLUMN 1 -------------------------->

> 044 

<!------------------- END SLIDE 044 it -------------------------->

<!----------------- BEGIN SLIDE 045 it -------------------------->

### 4.1. ServletContextListener


<!----------------- COLUMN 1 -------------------------->

> 045




Un *context listener*  , è un elemento utile in generale per eseguire ogni operazione di "inizializzazione globale" (non specifica per una particolare servlet) della web application.    

Oggetti che implementano l'interfaccia **ServletContextListener** possono essere associati all'applicazione dichiarandoli opportunamente nel deployment descriptor (web.xml).      

I due metodi `contextInitialized` e `contextDestroyed` di questi oggetti vengono chiamati rispettivamente all'avvio e alla chiusura della web application.  

I metodi possono, tra l'altro, leggere modificare il **ServletContext** che verrà poi passato a tutte le servlet in fase di esecuzione. 

<!------------------- END SLIDE 045 it -------------------------->

<!----------------- BEGIN SLIDE 046 it -------------------------->

####  Esempio


<!----------------- COLUMN 1 -------------------------->

> 046




```java
public class ContextInitializer implements ServletContextListener {

 public void contextInitialized(ServletContextEvent sce) {        
   //inizializziamo qualche variabile globale (di contesto)..
    sce.getServletContext().setAttribute("appID", 1);      
 }

 public void contextDestroyed(ServletContextEvent sce) {        

 }
}
```

```xml
<listener>
 <listener-class>it.univaq.f4i.iw.examples.ContextInitializer</listener-class>  
</listener>  
```

 


<!----------------- COLUMN 2 -------------------------->



Questo context listener esegue inizializza alcune variabili di contesto all'avvio della web application, inserendole come attributi nel ServletContext.        

Le singole servlet potranno prelevarle semplicemente con un'istruzione del tipo `getServletContext().getAttribute("appID")`      

Perché il listener sia elaborato, è sufficiente   **aggiungere il frammento in basso, che ne specifica la classe, nel web.xml**. 

<!------------------- END SLIDE 046 it -------------------------->

<!----------------- BEGIN SLIDE 047 it -------------------------->

### 4.2. Filter


<!----------------- COLUMN 1 -------------------------->

> 047




Un *filtro* è un elemento delle applicazioni web che permette di **modificare "al volo" le informazioni in input** (*HTTPServletRequest*) **o in output** (*HTTPServletResponse*) di una o più servlet.  

Oggetti che implementano l'interfaccia **Filter** possono essere associati all'applicazione dichiarandoli opportunamente nel deployment descriptor (web.xml).     

I metodi `init`, `destroy`  di questi oggetti vengono chiamati rispettivamente all'avvio e alla chiusura della web application.  

Il metodo  `doFilter`  viene invece invocato **a ogni richiesta** sulle servlet per le quali il filtro è configurato, e riceve gli oggetti request, response e una  *FilterChain*.

È possibile **sostituire gli oggetti request e/o response**  con implementazioni speciali per controllare l'input/output.   

È necessario **chiamare il metodo `doFilter` della FilterChain**. 

<!------------------- END SLIDE 047 it -------------------------->

<!----------------- BEGIN SLIDE 048 it -------------------------->

####  Esempio


<!----------------- COLUMN 1 -------------------------->

> 048



```java
public class EmailObfuscatorFilter implements Filter {        
 private FilterConfig config = null;      
 public void init(FilterConfig filterConfig) throws ServletException {            
  this.config = filterConfig;    
 }

 public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {                      
  chain.doFilter(request, response);      
 }
 
 public void destroy() {    
  config = null;   
 }
}
```

```xml
<filter>  
 <filter-name>emailfilter</filter-name>      
 <filter-class>EmailObfuscatorFilter</filter-class>      
</filter>  
<filter-mapping>  
 <filter-name>emailfilter</filter-name>      
 <url-pattern>**</url-pattern>    
 <dispatcher>REQUEST</dispatcher>    
</filter-mapping>  
```

 


<!----------------- COLUMN 2 -------------------------->



Questo filtro non fa assolutamente nulla: si limita a chiamare il `doFilter` sulla *FilterChain*.

Perché il filtro sia elaborato, è sufficiente **aggiungere il frammento in basso, che ne specifica la classe e gli url pattern associati, nel web.xml**.

> Si vedano le applicazioni di esempio: Java\_Example\_Emailfilter 

<!------------------- END SLIDE 048 it -------------------------->

<!----------------- BEGIN SLIDE 049 it -------------------------->

## 5. Riferimenti


<!----------------- COLUMN 1 -------------------------->

> 049




**Servlet API: Java EE 8**    
https://javaee.github.io/javaee-spec/javadocs/ 

**Servlet API: Jakarta EE 9**      
https://jakarta.ee/specifications/platform/9/apidocs/jakarta/servlet/package-summary.html    

**Servlet Tutorial (legacy)**      
https://docs.oracle.com/javaee/7/tutorial/servlets.htm

**JDBC Tutorial**   
http://docs.oracle.com/javase/tutorial/jdbc

**Apache Tomcat**     
https://tomcat.apache.org 

<!------------------- END SLIDE 049 it -------------------------->

<!----------------- BEGIN SLIDE 050 it -------------------------->

## 6. Esempi di codice


<!----------------- COLUMN 1 -------------------------->

> 049




Di seguito trovate un elenco dei principali esempi di codice mostrati o sviluppati durante le lezioni. Questi esempi sono tutti disponibili su GitHub, all'indirizzo [https://github.com/orgs/WebEngineering-Univaq], e sono *parte integrante* delle lezioni stesse, in quanto mostrano l'effettivo uso delle nozioni illustrate in aula e riportate su questa documentazione (dove, quando possibile, troverete dei riferimenti a questi esempi).

La lista che segue può non essere sempre aggiornata: nel repository potrete spesso trovare utili nuovi esempi appena sviluppati.

**Progetti template**

- Java_WebApp_Base_T10    
*Progetto di base per un'applicazione Web Java con Tomcat 10*

- Java_WebApp_Base_T9    
*Progetto di base per un'applicazione Web Java con Tomcat 9*

**Servlet di base**

- Java\_Example\_Servlet    
*Esempio base di servlet* 

- Java\_Example\_Servlet\_Fwk    
*L'esempio base di servlet riscritto utilizzando il framework di utilità del corso*

**Generazione delle risposte**

- Java\_Example\_Downloader    
*Esempio di download di dati binari generati tramite servlet* 

- Java\_Example\_Imager    
*Generazione di immagini dinamiche con le servlet*

**Gestione delle richieste**

- Java\_Example\_Post\_Redirect\_Get    
*Submission POST più sicure con il modello P-R-G*

- Java\_Example\_Servlet\_Multipart\_7    
*Decodifica delle richieste multipart con le servlet*

- Java\_Example\_Uploader    
*Un semplice repository di file sviluppato con le servlet* 

<!------------------- END SLIDE 050 it -------------------------->

<!----------------- BEGIN SLIDE 051 it -------------------------->


<!----------------- COLUMN 1 -------------------------->

> 051

**Sessioni**

- Java\_Example\_Servlet\_Sessions    
*Gestione di base delle sessioni con le servlet* 

- Java\_Example\_Login    
*Processo dettagliato di login/logout e gestione della sicurezza delle sessioni con le servlet*

- Java\_Example\_Login\_Middleware    
*Gestione dell'accesso e delle sessioni con le servlet e i filtri*

**Database (modello)**

- Java\_Example\_Servlet\_Database    
*Uso dei database nelle servlet Java*

**Modelli (Visualizza)**

- Java\_Example\_Templates    
*Esempi base per il motore di template Freemarker utilizzato nelle servlet*

- Java\_Example\_Templates\_Fwk    
*Il motore di template Freemarker utilizzato all'interno di un semplice framework di rendering*

**Argomenti avanzati**

- Java\_Example\_Ajax\_Pager\_Async    
*Paging lato server e lato client con servlet e javascript*

- Java\_Example\_Emailfilter    
*Uso di filtri per mascherare gli indirizzi e-mail nell'output dell'applicazione*

**Applicazioni web complete**

- Java\_Example\_Newspaper\_DAO    
*L'esempio del Newspaper, che mostra l'intero framework sviluppato nel corso* 

<!------------------- END SLIDE 051 it -------------------------->
