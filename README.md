    #include <SPI.h> // spI library
    #include <Ethernet.h>  //ethernet library
     
    int i=0;
     
    byte mac[] = { 0x90, 0xa2, 0xda, 0x0d, 0x4b, 0xdc }; //physical mac address
    byte ip[] = { 192, 168, 1, 10 }; // ip in lan
    byte gateway[] = { 192, 168, 1, 1 }; // internet access via router
    byte subnet[] = { 255, 255, 255, 0 }; //subnet mask
    EthernetServer server(80); //server port
     
    String readString;   
     
    void setup(){
     
      pinMode(6, OUTPUT); //pin selected to control
      pinMode(7, OUTPUT);
      pinMode(8, OUTPUT);
      pinMode(9, OUTPUT);    //start Ethernet
      Ethernet.begin(mac, ip, gateway, subnet);
      server.begin();
      //enable serial data print
      Serial.begin(9600);
      Serial.println("server LED test 1.0"); // so I can keep track of what is loaded
    }
     
    void loop(){
      // Create a client connection
      EthernetClient client = server.available();
      if (client) {
        while (client.connected()) {
          if (client.available()) {
            char c = client.read();
     
            //read char by char HTTP request
            if (readString.length() < 100) {
     
              //store characters to string
              readString += c;
              //Serial.print(c);
            }
     
            //if HTTP request has ended
            if (c == '\n') {if(readString.indexOf("?wireless") >0)//checks for on
              {i=1;}
               
              if(readString.indexOf("?gps") >0)//checks for off
            { i=2; }
               if(readString.indexOf("?sound") >0){i=3; }//code sound here
           if(readString.indexOf("?mind") >0){ i=4;}//code mind here 
         if(readString.indexOf("?face") >0){i=5; } // code face here
         if(readString.indexOf("?maze") >0){ i=6;} // code maze here
          if(readString.indexOf("?end") >0){i=0;digitalWrite(6, LOW);digitalWrite(7, LOW);digitalWrite(8, LOW);digitalWrite(9, LOW);}
              //clearing string for next read
              
             
            if(readString.indexOf("?up") >0){digitalWrite(6, HIGH);digitalWrite(7, LOW);digitalWrite(8, LOW);digitalWrite(9, HIGH);}
          if(readString.indexOf("?down") >0){digitalWrite(6, LOW);digitalWrite(7, HIGH);digitalWrite(8, HIGH);digitalWrite(9, LOW);}
          if(readString.indexOf("?left") >0){digitalWrite(6, HIGH);digitalWrite(7, LOW);digitalWrite(8, HIGH);digitalWrite(9, LOW);}
        if(readString.indexOf("?right") >0){digitalWrite(6, LOW);digitalWrite(7, HIGH);digitalWrite(8, LOW);digitalWrite(9, HIGH);}
         if(readString.indexOf("?end") >0){i=0;digitalWrite(6, LOW);digitalWrite(7, LOW);digitalWrite(8, LOW);digitalWrite(9, LOW);}   
         if(readString.indexOf("?stop") >0){digitalWrite(6, LOW);digitalWrite(7, LOW);digitalWrite(8, LOW);digitalWrite(9, LOW);} 
               
           
              //clearing string for next read
              readString="";
     
              ///////////////
              Serial.println(readString); //print to serial monitor for debuging
     
             // welcome Screan
              if(i==0)
              {client.println("HTTP/1.1 200 OK"); //send new page
              client.println("Content-Type: text/html");
              client.println();
     
              client.println("<HTML><HEAD>");
              
              
              client.println("<TITLE>ARES-ENAYLIUS</TITLE>");
              client.println("</HEAD>");
              client.println("<BODY>");
              client.println("<H1 ALIGN='CENTER'>ARES-ENYALIUS</H1>");
              client.println("<hr>");
              client.println("<br>");
             
              client.println("<FONT FACE='IMPACT' SIZE='6'> <U>Select Feature: </U></FONT>");
              client.println("<FONT COLOR='#008080'>&nbsp&nbsp&nbsp&nbsp&nbsp<a href=\"/?wireless\"\"><FONT><INPUT TYPE='submit' VALUE='Wireless Control'COLOR='YELLOW'</FONT></a><br>"); 
              client.println("&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp &nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp<a href=\"/?gps\"\"><INPUT TYPE='submit' VALUE='GPS Guided'></a><br>");   
         client.println("&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp &nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp <a href=\"/?sound\"\"><INPUT TYPE='submit' VALUE='Sound Processing'></a><br>");  
       client.println(" &nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp &nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp<a href=\"/?mind\"\"><INPUT TYPE='submit' VALUE='Mind Control'></a><br>");
       client.println(" &nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp &nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp <a href=\"/?face\"\"><INPUT TYPE='submit' VALUE='Face Recognition'></a><br>");
       client.println("&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp &nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp <a href=\"/?maze\"\"><INPUT TYPE='submit' VALUE='Maze Solving & Learner'></a><br>   ");
       client.println(" <br><br><br><br><br><br><br> <br><br><br><br><a href=\"/?end\"\"><INPUT TYPE='submit' VALUE='END'></a>");
      
       
         client.println("<P ALIGN='right'><FONT FACE='IMPACT' SIZE='4'> Designed By </FONT>    </P>");
              client.println("<P ALIGN='right'><FONT FACE='IMPACT' SIZE='6'> EAGLES </FONT>    </P>");
     
              client.println("</BODY></HTML>");
              
     
              delay(1);
              //stopping client
              client.stop();
              }
              
              
            if(i==1) // Wireless Page
          {
            client.println("HTTP/1.1 201 OK"); //send new page
              client.println("Content-Type: text/html");
              client.println();
   
              client.println("<HTML><HEAD>");
              
              
              client.println("<TITLE>ARES-ENAYLIUS</TITLE>");
              client.println("</HEAD>");
              client.println("<BODY>");
              client.println("<H1 ALIGN='CENTER'>Wireless Control & Live Broadcast</H1>");
              client.println("<hr>");
              client.println("<br>");
          
          client.println(" <P><FONT FACE='IMPACT' SIZE='6'><U> Direction </U></FONT></P>");
       client.println(" <iframe ALIGN='right'src='http://192.168.1.1/' width='50%' height='50%'></iframe>");
       client.println("<TABLE WIDTH='25%' BGCOLOR='#FEDCBA'><TR> ");
       client.println("<TD COLSPAN='3'ALIGN='CENTER'><a href=\"/?up\"\"><INPUT TYPE='submit' VALUE='UP'></a><br></TD></TR> ");
       client.println(" <TR><TD ALIGN='LEFT'><a href=\"/?left\"\"><INPUT TYPE='submit' VALUE='LEFT'></a></TD>");
       client.println("<TD ALIGN='center'><a href=\"/?stop\"\"><INPUT TYPE='submit' VALUE='STOP'></a></TD><TD ALIGN='right'><a href=\"/?right\"\"><INPUT TYPE='submit' VALUE='RIGHT'></a></TD></TR><TR>");
       client.println("<TD COLSPAN='3'ALIGN='CENTER'><a href=\"/?down\"\"><INPUT TYPE='submit' VALUE='DOWN'></a><br></TD></TR></TABLE>");
       client.println(" <P><FONT FACE='IMPACT' SIZE='6'> <U>Camera </U></FONT></P>");
          
          
          client.println(" <TABLE WIDTH='25%' BGCOLOR='#FEDCBA'><TR>");
       client.println("<TD COLSPAN='2'ALIGN='CENTER'><a href=\"/?up\"\"><INPUT TYPE='submit' VALUE='UP'></a><br>  </TD></TR> ");
       client.println(" <TR><TD ALIGN='LEFT'><a href=\"/?left\"\"><INPUT TYPE='submit' VALUE='LEFT'></a></TD>");
       client.println("<TD ALIGN='right'><a href=\"/right\"\"><INPUT TYPE='submit' VALUE='RIGHT'></TD></TR><TR> ");
          
          client.println(" <TD COLSPAN='2'ALIGN='CENTER'><a href=\"/?down\"\"><INPUT TYPE='submit' VALUE='DOWN'></a><br></TD></TR></TABLE> <br>");
       client.println(" <a href=\"/?end\"\"><INPUT TYPE='submit' VALUE='END'></a>");
       client.println(" <FONT COLOR='#008080'><br><br><br><br><br> ");
       
          client.println("<P ALIGN='right'><FONT FACE='IMPACT' SIZE='4'> Designed By </FONT>    </P>");
              client.println("<P ALIGN='right'><FONT FACE='IMPACT' SIZE='6'> EAGLES </FONT>    </P>");
     
              client.println("</BODY></HTML>");
          
          delay(1);
          client.stop();}
     
              ///////////////////// control arduino pin
          
              
              if(i==2) // gps guided
          {
            client.println("HTTP/1.1 201 OK"); //send new page
              client.println("Content-Type: text/html");
              client.println();
   
              client.println("<HTML><HEAD>");
              
              
              client.println("<TITLE>ARES-ENAYLIUS</TITLE>");
              client.println("</HEAD>");
              client.println("<BODY>");
              client.println("<H1 ALIGN='CENTER'>GPS Guided</H1>");
              
               //building the page
             
       client.println(" <br><br><br><br> <br><br><br><br><br><br><br><br><br><br><br> <br><br><br><br> <a href=\"/?end\"\"><INPUT TYPE='submit' VALUE='END'></a>");
       client.println(" <FONT COLOR='#008080'><br><br><br><br><br> ");
       
          client.println("<P ALIGN='right'><FONT FACE='IMPACT' SIZE='4'> Designed By </FONT>    </P>");
              client.println("<P ALIGN='right'><FONT FACE='IMPACT' SIZE='6'> EAGLES </FONT>    </P>");
     
              client.println("</BODY></HTML>");
          
          delay(1);
          client.stop();}
              
      
               
              if(i==3) // sound processing
          {
            client.println("HTTP/1.1 201 OK"); //send new page
              client.println("Content-Type: text/html");
              client.println();
   
              client.println("<HTML><HEAD>");
              
              
              client.println("<TITLE>ARES-ENAYLIUS</TITLE>");
              client.println("</HEAD>");
              client.println("<BODY>");
              client.println("<H1 ALIGN='CENTER'>Sound Processing</H1>");
              
               //building the page
             
       client.println("  <br><br><br><br> <br><br><br><br><br><br><br><br><br><br><br> <br><br><br><br><a href=\"/?end\"\"><INPUT TYPE='submit' VALUE='END'></a>");
       client.println(" <FONT COLOR='#008080'><br><br><br><br><br> ");
       
          client.println("<P ALIGN='right'><FONT FACE='IMPACT' SIZE='4'> Designed By </FONT>    </P>");
              client.println("<P ALIGN='right'><FONT FACE='IMPACT' SIZE='6'> EAGLES </FONT>    </P>");
     
              client.println("</BODY></HTML>");
          
          delay(1);
          client.stop();}
              
              
               
              if(i==4) // Mind Control 
          {
            client.println("HTTP/1.1 201 OK"); //send new page
              client.println("Content-Type: text/html");
              client.println();
   
              client.println("<HTML><HEAD>");
              
              
              client.println("<TITLE>ARES-ENAYLIUS</TITLE>");
              client.println("</HEAD>");
              client.println("<BODY>");
              client.println("<H1 ALIGN='CENTER'>Mind Control </H1>");
              
               //building the page
             
       client.println(" <br><br><br><br> <br><br><br><br><br><br><br><br><br><br><br> <br><br><br><br> <a href=\"/?end\"\"><INPUT TYPE='submit' VALUE='END'></a>");
       client.println(" <FONT COLOR='#008080'><br><br><br><br><br> ");
       
          client.println("<P ALIGN='right'><FONT FACE='IMPACT' SIZE='4'> Designed By </FONT>    </P>");
              client.println("<P ALIGN='right'><FONT FACE='IMPACT' SIZE='6'> EAGLES </FONT>    </P>");
     
              client.println("</BODY></HTML>");
          
          delay(1);
          client.stop();}
              
              
               
               
              if(i==5) // Face Recognition
          {
            client.println("HTTP/1.1 201 OK"); //send new page
              client.println("Content-Type: text/html");
              client.println();
   
              client.println("<HTML><HEAD>");
              
              
              client.println("<TITLE>ARES-ENAYLIUS</TITLE>");
              client.println("</HEAD>");
              client.println("<BODY>");
              client.println("<H1 ALIGN='CENTER'>Face Recognition</H1>");
              
               //building the page
             
       client.println(" <br><br><br><br> <br><br><br><br><br><br><br><br><br><br><br> <br><br><br><br> <a href=\"/?end\"\"><INPUT TYPE='submit' VALUE='END'></a>");
       client.println(" <FONT COLOR='#008080'><br><br><br><br><br> ");
       
          client.println("<P ALIGN='right'><FONT FACE='IMPACT' SIZE='4'> Designed By </FONT>    </P>");
              client.println("<P ALIGN='right'><FONT FACE='IMPACT' SIZE='6'> EAGLES </FONT>    </P>");
     
              client.println("</BODY></HTML>");
          
          delay(1);
          client.stop();}
          
          
          
              
              if(i==6) // Maze Solving and Learner
          {
            client.println("HTTP/1.1 201 OK"); //send new page
              client.println("Content-Type: text/html");
              client.println();
   
              client.println("<HTML><HEAD>");
              
              
              client.println("<TITLE>ARES-ENAYLIUS</TITLE>");
              client.println("</HEAD>");
              client.println("<BODY>");
              client.println("<H1 ALIGN='CENTER'>Maze Solving and Learner</H1>");
              
              //building the page
             
       client.println(" <br><br><br><br> <br><br><br><br><br><br><br><br><br><br><br> <br><br><br><br> <a href=\"/?end\"\"><INPUT TYPE='submit' VALUE='END'></a>");
       client.println(" <FONT COLOR='#008080'><br><br><br><br><br> ");
       
          client.println("<P ALIGN='right'><FONT FACE='IMPACT' SIZE='4'> Designed By </FONT>    </P>");
              client.println("<P ALIGN='right'><FONT FACE='IMPACT' SIZE='6'> EAGLES </FONT>    </P>");
     
              client.println("</BODY></HTML>");
          
          delay(1);
          client.stop();}
          
          
          // if(readString.indexOf("? each button inside every page ") >0){code here } // what inside every page and its code
              
              }
     
            
          }
        }
      }
    }

