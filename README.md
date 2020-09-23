<div align="center">

## Online School Registration Java Servlet


</div>

### Description

Simple example of a java servlet. This is a "tone-downed" version of a servlet I created for a school in the South. I figured maybe someone out there may get some use out of it, or maybe not. If you do, have fun with it. If you want use this, remember to create your DBMS (ie, MSACCESS).
 
### More Info
 


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Steven Jacobs](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/steven-jacobs.md)
**Level**          |Intermediate
**User Rating**    |5.0 (15 globes from 3 users)
**Compatibility**  |Java \(JDK 1\.1\)
**Category**       |[Miscellaneous](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/miscellaneous__2-57.md)
**World**          |[Java](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/java.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/steven-jacobs-online-school-registration-java-servlet__2-1829/archive/master.zip)

### API Declarations

<HTML>
<HEAD>
<META HTTP-EQUIV="Content-Type" CONTENT="text/html; charset=windows-1252">
<TITLE>
U of M Online Registration Servlet
</TITLE>
</HEAD>
<BODY>
<H2>U of M Online Registration Servlet</H2>
 launches servlet OnlineRegsServlet <BR>
 <FORM action=http://localhost:8080/servlet/untitled28.OnlineRegsServlet method ="POST">
 *(Required Fields)<BR>
 *Name:   <INPUT TYPE = text NAME = Name><BR>
 *Class Standing:<INPUT TYPE = text NAME = Class_Standing><BR>
 *Term:   <INPUT TYPE = text NAME = Term><BR>
 *Year:   <INPUT TYPE = text NAME = Year><BR>
 </PRE>
 <P>Select the class/classes you wish to take this term.<BR>
 <INPUT TYPE = CHECKBOX NAME = Class1 VALUE = Class1>COMP 1900<BR>
 <INPUT TYPE = CHECKBOX NAME = Class2 VALUE = Class2>COMP 2150<BR>
 <INPUT TYPE = CHECKBOX NAME = Class3 VALUE = Class3>COMP 3160<BR>
 <INPUT TYPE = CHECKBOX NAME = Class4 VALUE = Class4>COMP 4030<BR>
 <INPUT TYPE = CHECKBOX NAME = Class5 VALUE = Class5>COMP 4990<BR>
 <INPUT TYPE = CHECKBOX NAME = Class6 VALUE = Class6>COMP 6320<BR>
 </P>
 <INPUT TYPE = SUBMIT Value="Submit Classes">
 <INPUT TYPE = RESET>
 </FORM>
</BODY>
</HTML>


### Source Code

///////////////////////////////////////////////////////////////////////////
//Title: Online Registration Servlet
//Author: Steven Jacobs
//Dev. Tools: JBuilder3..JDK 1.2
//Description: This is a mock-up version of an online registration servlet
//    for any university. It is similar to an online family reservation
//    servlet I created about 7 months ago. I thought I would take about
//    an hour to do this and share it with you. Maybe you can get
//    something out of it. Please improve.
////////////////////////////////////////////////////////////////////////////
package untitled28;
import javax.servlet.*;
import javax.servlet.http.*;
import java.io.*;
import java.util.*;
import java.sql.*;
public class OnlineRegsServlet extends HttpServlet
{
 private Statement conStatement = null;
 private Connection conDatabase = null;
 private String odbcURL = "jdbc:odbc:RegsBook";
 String name;
 String classStanding;
 String term;
 String year;
 String class1;
 String class2;
 String class3;
 String class4;
 String class5;
 String class6;
 PrintWriter outWrite;
 //Initialize global variables
 public void init(ServletConfig config) throws ServletException
 {
 super.init(config);
 try
 {
  Class.forName("sun.jdbc.odbc.JdbcOdbcDriver");
  conDatabase = DriverManager.getConnection(odbcURL, " ", " ");
 }
 catch (Exception e)
 {
  e.printStackTrace();
  conDatabase = null;
 }
 }
 public void doPost(HttpServletRequest request, HttpServletResponse response)
    throws ServletException, IOException
 {
 //String name,classStanding,term,year,class1,class2,class3,class4,class5,class6;
 name = request.getParameter("Name");
 classStanding = request.getParameter("Class_Standing");
 term = request.getParameter("Term");
 year = request.getParameter("Year");
 class1 = request.getParameter("Class1");
 class2 = request.getParameter("Class2");
 class3 = request.getParameter("Class3");
 class4 = request.getParameter("Class4");
 class5 = request.getParameter("Class5");
 class6 = request.getParameter("Class6");
 outWrite = response.getWriter();
 response.setContentType("text/html");
 if (name.equals("") || classStanding.equals("") || term.equals("") ||
  year.equals(""))
 {
  outWrite.println("<H3>All the fields '*' fields must be filled out</H3>");
  outWrite.close();
  return;
 }
 boolean insertSuccess = inputInformation(
 "'" + name + "','" + classStanding + "','" + term + "','" + year +
 "','" + (class1 != null ? "yes" : "no") + "','" +
 (class2 != null ? "yes" : "no") + "','" +
 (class3 != null ? "yes" : "no") + "','" +
 (class4 != null ? "yes" : "no") + "','" +
 (class5 != null ? "yes" : "no") + "','" +
 (class6 != null ? "yes" : "no") + "'");
 if (insertSuccess)
 {
 outWrite.print("<H2> "+ name + " is registered for the " + term + " term " +
    ", " + year + "</H2>");
 }
 else
  outWrite.print("<H2>Server error...try again later.</H2>");
}
private boolean inputInformation(String insertInfo)
{
 try
 {
 conStatement = conDatabase.createStatement();
 conStatement.execute("INSERT INTO RegTable values(" + insertInfo + ");");
 conStatement.close();
 }
 catch(Exception e)
 {
 System.err.println("Something is wrong..I cannot add any new entry " +
 "HELP ME");
 e.printStackTrace();
 return false;
 }
 return true;
}
public void destroy()
{
 try
 {
 conDatabase.close();
 }
 catch (Exception e)
 {
 System.err.println("Cannot close the database");
 }
}
 //Get Servlet information
 public String getServletInfo()
 {
 return "untitled28.OnlineRegsServlet Information";
 }
}

