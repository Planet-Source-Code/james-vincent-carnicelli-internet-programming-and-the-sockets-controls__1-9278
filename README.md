﻿<div align="center">

## Internet Programming and the Sockets Controls


</div>

### Description

Here's a VB programmer's introduction to developing Internet (TCP/IP) applications. We'll build a trivial web browser to start with Then we'll look at some of the issues you should consider when building client/server applications. We'll even build a simple one.
 
### More Info
 


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[James Vincent Carnicelli](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/james-vincent-carnicelli.md)
**Level**          |Beginner
**User Rating**    |5.0 (55 globes from 11 users)
**Compatibility**  |VB 3\.0, VB 4\.0 \(16\-bit\), VB 4\.0 \(32\-bit\), VB 5\.0, VB 6\.0, VB Script, ASP \(Active Server Pages\) 
**Category**       |[Internet/ HTML](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/internet-html__1-34.md)
**World**          |[Visual Basic](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/visual-basic.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/james-vincent-carnicelli-internet-programming-and-the-sockets-controls__1-9278/archive/master.zip)





### Source Code

<P><FONT SIZE="+2" COLOR="#000066"><B> Table of Contents </B></FONT>
<LI><A HREF="#preface">Preface</A>
<LI><A HREF="#clientserver">Client / Server Concepts</A>
<LI><A HREF="#introduction">Introduction to Internet Programming </A>
<LI><A HREF="#package">The Sockets Package</A>
<LI><A HREF="#browser">Build a Basic Web Browser</A>
<LI><A HREF="#csapp">Build a Complete Client / Server App</A>
<LI><A HREF="#conclusion">Conclusion</A>
<A NAME="preface">
<P><FONT SIZE="+2" COLOR="#000066"><B> Preface </B></FONT>
<BR>In less than a decade, TCP/IP - the Internet - has emerged from the cacophony of networking protocols as the
undisputed winner. So many information protocols, from HTTP (web) to IRC (chat), have been developed to offer all
manner of electronic content. With TCP/IP dominance secured, many companies with in-house IT staffs are moving
towards developing their own <A HREF="#clientserver">client/server</A> applications using home-grown or off the
shelf Internet protocols. This article can help you leap on board this roaring technology train.
<P>Most Internet programmers developing for windows use some form or another of the Winsock API. You may already be
aware of this API's infamy as a difficult one to master. As a VB programmer, you may also be aware of the fact that
VB ships with a Winsock control that enwraps the deeply confusing Winsock API in a slightly less confusing package.
But it's still confusing to most new programmers. It's also known for being buggy. It also doesn't help that all
the functionality for developing clients and servers is lumped into one control, which leaves many programmers with
little clue about how and when to use its features.
<P>I recently developed a suite of controls called "Sockets" to build on the virtues of the Winsock control while
masking most of its inadequacies. It's easier to use and offers sophisticated features like multi-connection
management and message broadcasting. This code samples in this article will be built around the Sockets package.
<BLOCKQUOTE>
<P>Note: You can download the Sockets package from Planet Source Code. Search here for the posting's title: "<A
TARGET="_new"
HREF="http://www.planet-source-code.com/vb/scripts/BrowseCategoryOrSearchResults.asp?lngWId=1&txtCriteria=Simple,+cl
ean+client+server+socket+controls">Simple, clean client/server socket controls</A>". Be sure to include the
"Sockets" component ("Sockets.OCX") in any projects you create to try out the code samples. You can register the
control so it appears in VB's component list from the Start | Run menu item using "<TT>regsvr32 <FONT
COLOR="#993333">&lt;path_to_ocx&gt;</FONT>\sockets.ocx</TT>".
</BLOCKQUOTE>
<P>If you're already familiar with client/server and sockets concepts, you can skip right to the <A
HREF="#package">Sockets Package</A> section for information specific to the controls used and how to use them.
<A NAME="clientserver">
<P><FONT SIZE="+2" COLOR="#000066"><B> Client / Server Concepts</B></FONT>
<BR>Before we begin talking about Internet programming, let's give a brief introduction to the client/server
concept.
<P>The "client/server" concept is a fundamentally simple one. Some automated entity - a program, component,
machine, or whatever - is available to process information on behalf of other remote entities. The former is called
a "server", the latter a "client". The most popular client/server application today is the World Wide Web. In this
case, the servers are all those web servers companies like Yahoo and Microsoft run to serve up web pages. The
clients are the web browsers we use to get at their web sites.
<P>There are a number of other terms commonly used in discussing the client/server concept. A "<B>connection</B>"
is a completed "pipeline" through which information can flow between a single client and a single server. The
client is always the connection requestor and the server is always the one listening for and accepting (or
rejecting) such requests. A "<B>session</B>" is a continuous stream of processing between a client and server.
That duration is not necessarily the same as the duration of one connection, nor does a session necessarily involve
only one simultaneous connection. "<B>Client interconnection</B>" is what a server does to facilitate information
exchange among multiple clients. A chat program is a good example. Usually, nothing can be done with a given
message until all of it is received. A "<B>message</B>", in this context, is any single piece of information that's
sent one way or the other through a connection. Messages are typically single command requests or server responses.
In most cases, a message can't be used until all of it is received. A "<B>remote procedure</B>" is simply a
procedure that a client asks a server to execute on its behalf, which usually involves one command message going to
the server and one response message coming back from it. Using an FTP client to rename a file on a server is an
example. An "<B>event</B>" is the converse of a remote procedure call: the server sends this kind of message to
the client, which may or may not respond to.
<P>As programmers, we generally take for granted that a given function call does not return until it is done
executing. Why would we want it to, otherwise? Having the code that calls a function wait until it is done is
called "<B>synchronous</B>". The alternative - allowing the calling code to continue on even before the function
called is done - is called "<B>asynchronous</B>". Different client/server systems employ each of these kinds of
procedure calling modes. Usually, an asynchronous client/server system will involve attaching unique, random
numbers to each message and having a response to a given message include that same number, which can be used to
differentiate among messages that may arrive out of their expected order. The main benefit to this sort of scheme
is that processing can continue on both sides without delays. Such systems are usually a bit complicated to create
and make the most of.
<P>There are plenty of other concepts related to the client/server concept, but this should suffice for starters.
<A NAME="introduction">
<P><FONT SIZE="+2" COLOR="#000066"><B> Introduction to Internet Programming </B></FONT>
<BR>As you might already have guessed, programming for the Internet is quintessentially client/server programming.
Your program can't connect to any other program using the Internet without that other program being an active
server. The feature that distinguishes Internet client/server systems from others is TCP/IP, which stands for
Transmission Connection Protocol / Internet Protocol. TCP/IP was developed as a generic communication protocol that
transcends the particular, lower-level network systems they rest on top of, like Ethernet LANs, phone lines, digital
cellular systems, and so on.
The Internet protocol - the IP in TCP/IP - is a complex packet-switching protocol in which messages sent through
connections are chopped up into "packets" - low-level messages our programs generally never need to directly see -
and sent across any number of physical connections to the other side of the Internet connection. These are
reassembled at the receiving end. Those packets may not arrive at the same time, though, and some may never arrive
at all. Internet phone and streaming video systems are fine with this sort of asynchronous communication, since
it's fast. Those programs use the "UDP" (User Datagram Protocol). For this article, we'll be dealing with the TCP,
in which these packets are properly assembled back into the original data stream at the receiving end, with a
guarantee that if the packets can get there, they will.
<P>Inernet programming is also often called "<B>sockets programming</B>", owing to the Berkley sockets API, one of
the first of its kind. Because programmers of sockets applications on windows use the "Winsock" API, it's also
often called by "<B>Winsock programming</B>". Winsock is simply an adaptation of the Berkley sockets API for
Windows.
<P>Most Internet client/server systems use sockets to interface with TCP/IP. A socket is an abstract representation
for a program of one end of an Internet connection. There are three basic kinds of sockets: client, server, and
listener. A server application will have a listener socket do nothing but wait for incoming connection requests.
That application will decide, when one arrives, whether or not to accept this request. If it accepts it, it will
actually bind that connection to a server socket. Most servers have many server sockets that can be allocated; at
least one for each active connection. The client application only needs a client socket. Either side can
disconnect, which simply breaks the connection on both sides.
<P>Once a connection is established, each side can send bytes of data to the other. That data will always arrive at
the other side in the same order it was sent. Both sides can be sending data at the same time, too. This is called
a "<B>data stream</B>". All data that gets sent between a client and server passes through this stream.
<P>Everything else that applies to the <A HREF="#clientserver">client/server</A> concept applies here as well, so
we'll dispense with the details and get right into Internet programming with the Sockets controls.
<A NAME="package">
<P><FONT SIZE="+2" COLOR="#000066"><B> The Sockets Package </B></FONT>
<BR>The Sockets package, which you can download via the link in the <A HREF="#preface">preface</A>, is a collection
of controls that simplify interfacing with the Winsock API and hence the Internet. There are controls for each of
the three types of sockets: client, server, and listener. There is also a control that combines one listener
socket and a bank of server sockets. This control hides the gory details of socket management that most servers
otherwise have to do themselves. A server that uses this control won't need to directly deal with the listener or
server sockets.
<P>We won't get deeply into the details of the Sockets package here. Let me encourage you to refer to "help.html",
the help file that came with the Sockets package you <A HREF="#preface">downloaded</A>.
<A NAME="browser">
<P><FONT SIZE="+2" COLOR="#000066"><B> Build a Basic Web Browser </B></FONT>
<BR>The HTTP protocol that drives the World Wide Web is surely the most used TCP/IP application. It's wonderful
that it should also be one of the easiest to master. We'll do this by building a simple web browser. It won't have
all the advanced features like WYSIWYG, scripting, and so on, but it will demonstrate the basic secrets behind HTTP.
<P>Before we get started, you'll need to make sure you have access to the web without the use of a proxy to get
through a firewall. If you're inside a corporate intranet, you may at least have access to your own company's web
servers. If you're not sure about all this or can't run the program we'll be building, consult your network
administrator.
<P>Now, let's start by creating our project and building a form. Our project needs to include the "Sockets"
component, which is the "Sockets.ocx" file that came with the Sockets package we downloaded. The form should look a
little something like this:
<P><CENTER><TABLE BGCOLOR="#CCCCCC" CELLSPACING="0" CELLPADDING="4" BORDER="2">
<TR><TD BGCOLOR="000066"><FONT COLOR="#FFFFFF"><B> Form1 </B></FONT></TD></TR>
<TR><TD><TABLE>
<TR>
  <TD>Url: </TD>
<TD><TABLE BGCOLOR="#FFFFFF" WIDTH="100" CELLSPACING="0" CELLPADDING="0" BORDER="1"><TR><TD><NOBR>&nbsp;
   <FONT SIZE="-1" COLOR="#CC6666"> Name = "Host" </FONT> </NOBR></TD></TR></TABLE></TD>
<TD><TABLE BGCOLOR="#FFFFFF" WIDTH="200" CELLSPACING="0" CELLPADDING="0" BORDER="1"><TR><TD><NOBR>&nbsp;
   <FONT SIZE="-1" COLOR="#CC6666"> Name = "Path" </FONT> </NOBR></TD></TR></TABLE></TD>
  <TD><TABLE CELLSPACING="0" CELLPADDING="0" BORDER="1"><TR><TD>&nbsp; Go &nbsp;</TD></TR></TABLE></TD>
 </TR><TR>
  <TD COLSPAN="4"><TABLE BGCOLOR="#FFFFFF" WIDTH="100%" CELLSPACING="0" CELLPADDING="4" BORDER="1"><TR><TD>
   <FONT SIZE="-1" COLOR="#CC6666"> Name = "Contents" </FONT>
   <BR>&nbsp; <BR>&nbsp; <BR>&nbsp;
   <BR>&nbsp; <BR>&nbsp; <BR>&nbsp;
<TABLE BGCOLOR="#FFFF99" CELLSPACING="0" CELLPADDING="2" BORDER="1" ALIGN="RIGHT"><TR><TD> CS </TD></TR></TABLE>
</TD></TR></TABLE></TD>
 </TR>
</TABLE></TD>
</TR>
</TABLE></CENTER>
<P>"CS" is a ClientSocket control. Be sure to give the button labeled "Go" the name "Go". Now enter the following
code in the form module:
<P><PRE>
<FONT COLOR="#000099">Private Sub</FONT> Go_Click()
&nbsp;&nbsp;&nbsp;&nbsp;Contents.Text = ""
&nbsp;&nbsp;&nbsp;&nbsp;CS.Connect Host.Text, 80
&nbsp;&nbsp;&nbsp;&nbsp;CS.Send "GET " & Path.Text & vbCrLf & vbCrLf
&nbsp;&nbsp;&nbsp;&nbsp;<FONT COLOR="#000099">While</FONT> CS.Connected
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<FONT COLOR="#000099">If</FONT> CS.BytesReceived > 0 <FONT
COLOR="#000099">Then</FONT>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Contents.SelText = CS.Receive
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<FONT COLOR="#000099">End If</FONT>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;DoEvents
&nbsp;&nbsp;&nbsp;&nbsp;<FONT COLOR="#000099">Wend</FONT>
<FONT COLOR="#000099">End Sub</FONT>
</PRE>
<P>Hard to believe it could be that easy, but it is. Try running this with <TT>Host</TT> =
"www.planet-source-code.com" and <TT>Path</TT> = "/vb/". Not surprisingly, this won't look as nice as it does in,
say, Internet Explorer, but that's because we're only retrieving what the server has to offer. We're not actually
reading what comes back to decide what to make of it. That's much harder. But the network interaction part is at
the heart of what your Internet programming effort will most often be about. This code could form the basis of a
program to grab information from one of your business partners' web sites to populate your own database: perhaps
the latest pricing and availability figures; or perhaps to get a car's blue book value from a search engine.
<P>Since this article isn't fundamentally about web browsers, we'll skip these sorts of details. Instead, we'll now
build a custom client / server application from scratch.
<A NAME="csapp">
<P><FONT SIZE="+2" COLOR="#000066"><B> Build a Complete Client / Server App </B></FONT>
<BR><FONT SIZE="+1" COLOR="#0066FF"><B> The Nature of the Beast </B></FONT>
<BR>We've talked about the <A HREF="#clientserver">client / server</A> concept and we've <A HREF="#browser">built a
web browser</A> to demonstrate a client. Let's now invent an Internet protocol of our own and build client and
server programs to implement it.
<P>Our application's purpose will be simple: to allow a number of different computers share some data variables in a
way that allows all of them to not only read and write those variables, but also to be aware of any changes to that
data by other computers as they happen.
<P>What sort of information protocol do we need to make this happen? Obviously, we'll want the clients interested
to be able to connect to a server that maintains the data. We'll keep it simple by not allowing any client to be
disconnected during a session. We'll want to require clients to log in at the beginning of the session. The
clients will need to be able to send commands to the server ("remote procedures") and get a response for each
command invocation. We'll allow communication to be asynchronous, meaning the client won't have to wait for a
response to a given command before continuing. We'll also need to have the server be able to trigger events the
client can make use of. Here are the messages our clients and server will need to be able to exchange:
<P><UL>
<LI>LogIn <FONT COLOR="#006600">&lt;user&gt;</FONT> <FONT COLOR="#006600">&lt;password&gt;</FONT>
<LI>LogInResult <FONT COLOR="#006600">&lt;true_or_false&gt;</FONT>
<LI>GetValue <FONT COLOR="#006600">&lt;name&gt;</FONT>
<LI>GetAllValues
<LI>SetValue <FONT COLOR="#006600">&lt;name&gt;</FONT> <FONT COLOR="#006600">&lt;value&gt;</FONT>
<LI>ValueEquals <FONT COLOR="#006600">&lt;name&gt;</FONT> <FONT COLOR="#006600">&lt;value&gt;</FONT>
<LI>ValueChanged <FONT COLOR="#006600">&lt;by_user&gt;</FONT> <FONT COLOR="#006600">&lt;name&gt;</FONT> <FONT
COLOR="#006600">&lt;value&gt;</FONT>
</UL>
<P>How will we represent a message? A message will begin with a message name (e.g., "GetValue") and will have zero
or more parameters. Each message will be followed by <TT>&lt;CR&gt;&lt;LF&gt;</TT>, the standard way Windows
programs represent a new line. We'll put a space after the message name and between each parameter. Because we've
given special meaning to the new-line character combination and the space character, we can't use them anywhere
within the message names or the parameters. What if a parameter contains one of these special character
combinations? Our protocol will include "metacharacters", or special combinations of characters that are meant to
represent other character combinations. Here are the characters and what we'll be replacing them with:
<P><TABLE>
 <TR><TD><LI>"<B>\</B>" </TD><TD> => "<B>\b</B>" </TD><TD> ("b" for "backslash") </TD></TR>
<TR><TD><LI>" " </TD><TD> => "<B>\s</B>" </TD><TD> ("s" for "space") </TD></TR>
<TR><TD><LI>vbCr </TD><TD> => "<B>\r</B>" </TD><TD> ("r" for "carriage return") </TD></TR>
<TR><TD><LI>vbLf </TD><TD> => "<B>\l</B>" </TD><TD> ("l" for "line feed") </TD></TR>
</TABLE>
<P>Note that we're even replacing the backslash (\) character with a metacharacter because we're also giving special
meaning to backslash as the start of a metacharacter representation.
<P><FONT SIZE="+1" COLOR="#0066FF"><B> The Code </B></FONT>
<BR>Let's create the project. As before, the project needs to include the "Sockets" component, which is the
"Sockets.ocx" file that came with the Sockets package we downloaded. Create two forms, called "Server" and
"Client". They should look like the following:
<P><CENTER><TABLE BGCOLOR="#CCCCCC" CELLSPACING="0" CELLPADDING="4" BORDER="2">
<TR><TD BGCOLOR="000066"><FONT COLOR="#FFFFFF"><B> Server </B></FONT></TD></TR>
<TR><TD><TABLE>
<TR>
<TD COLSPAN="4"><TABLE BGCOLOR="#FFFFFF" WIDTH="100%" CELLSPACING="0" CELLPADDING="4" BORDER="1"><TR><TD>
   <FONT SIZE="-1" COLOR="#CC6666"> Type = ListBox
   <BR>Name = "Connections" </FONT>
   <BR>&nbsp; <BR>&nbsp; <BR>&nbsp;
<TABLE BGCOLOR="#FFFF99" CELLSPACING="0" CELLPADDING="2" BORDER="1" ALIGN="RIGHT"><TR><TD> SSB </TD></TR></TABLE>
</TD></TR></TABLE></TD>
 </TR>
</TABLE></TD>
</TR>
</TABLE></CENTER>
<P><CENTER><TABLE BGCOLOR="#CCCCCC" CELLSPACING="0" CELLPADDING="4" BORDER="2">
<TR><TD BGCOLOR="000066"><FONT COLOR="#FFFFFF"><B> Client </B></FONT></TD></TR>
<TR><TD><TABLE>
<TR>
<TD ALIGN="RIGHT"><TABLE CELLSPACING="0" CELLPADDING="0" BORDER="1"><TR><TD>&nbsp; Start the Server
&nbsp;</TD></TR></TABLE></TD>
<TD COLSPAN="3" ALIGN="LEFT"><TABLE CELLSPACING="0" CELLPADDING="0" BORDER="1"><TR><TD>&nbsp; Launch Another Client
&nbsp;</TD></TR></TABLE></TD>
</TR><TR>
 <TD><TABLE BGCOLOR="#FFFFFF" CELLSPACING="0" CELLPADDING="0" BORDER="1"><TR><TD>&nbsp;
   <FONT SIZE="-1" COLOR="#CC6666"> Name = "VarName" </FONT> </TD></TR></TABLE></TD>
 <TD><TABLE BGCOLOR="#FFFFFF" CELLSPACING="0" CELLPADDING="0" BORDER="1"><TR><TD>&nbsp;
   <FONT SIZE="-1" COLOR="#CC6666"> Name = "VarValue" </FONT> </TD></TR></TABLE></TD>
  <TD><TABLE CELLSPACING="0" CELLPADDING="0" BORDER="1"><TR><TD>&nbsp; Set &nbsp;</TD></TR></TABLE></TD>
 </TR><TR>
  <TD COLSPAN="4"><TABLE BGCOLOR="#FFFFFF" WIDTH="100%" CELLSPACING="0" CELLPADDING="4" BORDER="1"><TR><TD>
   <FONT SIZE="-1" COLOR="#CC6666"> Type = ListBox
   <BR>Name = "VarList" </FONT>
   <BR>&nbsp; <BR>&nbsp; <BR>&nbsp;
<TABLE BGCOLOR="#FFFF99" CELLSPACING="0" CELLPADDING="2" BORDER="1" ALIGN="RIGHT"><TR><TD> CS </TD></TR></TABLE>
</TD></TR></TABLE></TD>
 </TR>
</TABLE></TD>
</TR>
</TABLE></CENTER>
<P>"CS" is a ClientSocket control. "SSB" is a ServerSocketBank control. We'll give the button labeled "Set" the
name "SetVar". We'll call the other two buttons on the client "StartServer" and "AnotherClient". Here's the code
for the server:
<P><CENTER><TABLE BGCOLOR="#FFFFDD" CELLSPACING="0" CELLPADDING="4" BORDER="1"><TR><TD>
<PRE>
Private VariableNames As Collection
Private Variables As Collection
<BR><BR><FONT COLOR="#009900">'Let's do something with this message</FONT>
Private Sub ProcessMessage(Socket, Message)
&nbsp;&nbsp;&nbsp;&nbsp;Dim i, Session
&nbsp;&nbsp;&nbsp;&nbsp;Set Session = Socket.ExtraTag
&nbsp;&nbsp;&nbsp;&nbsp;If Not Session("LoggedIn") _
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;And Message(0) <> "LogIn" Then Exit Sub
&nbsp;&nbsp;&nbsp;&nbsp;Select Case Message(0)
<BR><BR>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Case "LogIn"
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;If Message(2) = "pollywog" Then
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;SetItem Session,
"LoggedIn", True
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;SetItem Session,
"User", Message(1)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;SendMessage Socket,
"LogInResult", "True"
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Else
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;SetItem Session,
"LoggedIn", False
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;SendMessage Socket,
"LogInResult", "False"
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;End If
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;RefreshDisplay
<BR><BR>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Case "GetValue"
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;On Error Resume Next
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;i = Variables(Message(1))
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;On Error GoTo 0
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;SendMessage "ValueEquals", Message(1), i
<BR><BR>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Case "GetAllValues"
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;For i = 1 To VariableNames.Count
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;SendMessage Socket,
"ValueEquals", _
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Variable
Names(i), Variables(i)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Next
<BR><BR>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Case "SetValue"
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;SetItem VariableNames, Message(1),
Message(1)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;SetItem Variables, Message(1), Message(2)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;SSB.Broadcast "ValueChanged " & _
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Encode(Session("User")) & " " &
_
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Encode(Message(1)) & " " & _
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Encode(Message(2)) & vbCrLf
<BR><BR>&nbsp;&nbsp;&nbsp;&nbsp;End Select
End Sub
<BR><BR><FONT COLOR="#009900">'Refresh the list box of connections</FONT>
Private Sub RefreshDisplay()
&nbsp;&nbsp;&nbsp;&nbsp;Dim i As Integer
&nbsp;&nbsp;&nbsp;&nbsp;Connections.Clear
&nbsp;&nbsp;&nbsp;&nbsp;For i = 1 To SSB.MaxSocket
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;If SSB.IsInUse(i) Then
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Connections.AddItem i & vbTab &
SSB(i).ExtraTag("User")
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Else
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Connections.AddItem i & vbTab & "<not in
use>"
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;End If
&nbsp;&nbsp;&nbsp;&nbsp;Next
End Sub
<BR><BR><FONT COLOR="#009900">'Initialize everything and start listening</FONT>
Private Sub Form_Load()
&nbsp;&nbsp;&nbsp;&nbsp;Set VariableNames = New Collection
&nbsp;&nbsp;&nbsp;&nbsp;Set Variables = New Collection
&nbsp;&nbsp;&nbsp;&nbsp;SetItem VariableNames, "x", "x"
&nbsp;&nbsp;&nbsp;&nbsp;SetItem Variables, "x", 12
&nbsp;&nbsp;&nbsp;&nbsp;SetItem VariableNames, "y", "y"
&nbsp;&nbsp;&nbsp;&nbsp;SetItem Variables, "y", "ganlion"
&nbsp;&nbsp;&nbsp;&nbsp;SSB.Listen STANDARD_PORT
End Sub
<BR><BR><FONT COLOR="#009900">'A client just connected</FONT>
Private Sub SSB_Connected(Index As Integer, _
&nbsp;&nbsp;Socket As Object)
&nbsp;&nbsp;&nbsp;&nbsp;Dim Session
&nbsp;&nbsp;&nbsp;&nbsp;Set Session = New Collection
&nbsp;&nbsp;&nbsp;&nbsp;SetItem Session, "LoggedIn", False
&nbsp;&nbsp;&nbsp;&nbsp;SetItem Session, "User", ""
&nbsp;&nbsp;&nbsp;&nbsp;SetItem Session, "Buffer", ""
&nbsp;&nbsp;&nbsp;&nbsp;Set Socket.ExtraTag = Session
&nbsp;&nbsp;&nbsp;&nbsp;RefreshDisplay
End Sub
<BR><BR><FONT COLOR="#009900">'A client just disconnected</FONT>
Private Sub SSB_Disconnect(Index As Integer, _
&nbsp;&nbsp;Socket As Object)
&nbsp;&nbsp;&nbsp;&nbsp;RefreshDisplay
End Sub
<BR><BR><FONT COLOR="#009900">'A client sent message data</FONT>
Private Sub SSB_DataArrival(Index As Integer, _
&nbsp;&nbsp;Socket As Object, Bytes As Long)
&nbsp;&nbsp;&nbsp;&nbsp;Dim Message, Buffer
&nbsp;&nbsp;&nbsp;&nbsp;Buffer = Socket.ExtraTag("Buffer") & Socket.Receive
&nbsp;&nbsp;&nbsp;&nbsp;SetItem Socket.ExtraTag, "Buffer", Buffer
&nbsp;&nbsp;&nbsp;&nbsp;While ParseMessage(Buffer, Message)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;SetItem Socket.ExtraTag, "Buffer", Buffer
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ProcessMessage Socket, Message
&nbsp;&nbsp;&nbsp;&nbsp;Wend
End Sub
</PRE>
</TD></TR></TABLE></CENTER>
<P>The core of this code is the <TT>ProcessMessage</TT> subroutine. The message that's passed to it will be an
array of strings representing the message name and its parameters. This array is generated by the
<TT>ParseMessage</TT> routine, which we'll get to momentarily.
<P>Now here's the code for the client form's module:
<P><CENTER><TABLE BGCOLOR="#FFFFDD" CELLSPACING="0" CELLPADDING="4" BORDER="1"><TR><TD>
<PRE>
Private VariableNames As Collection
Private Variables As Collection
Private Buffer As String
Private User As String
<BR><BR><FONT COLOR="#009900">'Let's do something with this message</FONT>
Private Sub ProcessMessage(Socket, Message)
&nbsp;&nbsp;&nbsp;&nbsp;Dim i
&nbsp;&nbsp;&nbsp;&nbsp;Select Case Message(0)
<BR><BR>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Case "LogInResult"
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;If Message(1) = False Then
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;MsgBox "Login
denied"
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;CS.Disconnect
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Else
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;SetVar.Enabled =
True
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;SendMessage CS,
"GetAllValues"
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;End If
<BR><BR>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Case "ValueEquals"
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;SetItem VariableNames, Message(1),
Message(1)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;SetItem Variables, Message(1), Message(2)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;RefreshDisplay
<BR><BR>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Case "ValueChanged"
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;SetItem VariableNames, Message(2),
Message(2)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;SetItem Variables, Message(2), Message(3)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;RefreshDisplay
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;If Message(1) <> User Then
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;MsgBox Message(2) &
" was changed by " & Message(1)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;End If
<BR><BR>&nbsp;&nbsp;&nbsp;&nbsp;End Select
End Sub
<BR><BR><FONT COLOR="#009900">'Refresh the list box of variables</FONT>
Private Sub RefreshDisplay()
&nbsp;&nbsp;&nbsp;&nbsp;Dim i
&nbsp;&nbsp;&nbsp;&nbsp;VarList.Clear
&nbsp;&nbsp;&nbsp;&nbsp;For i = 1 To VariableNames.Count
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;VarList.AddItem VariableNames(i) & " = " & Variables(i)
&nbsp;&nbsp;&nbsp;&nbsp;Next
End Sub
<BR><BR><FONT COLOR="#009900">'Initialize everything and connect to the server</FONT>
Private Sub Form_Load()
&nbsp;&nbsp;&nbsp;&nbsp;Dim Host, Password
&nbsp;&nbsp;&nbsp;&nbsp;SetVar.Enabled = False
&nbsp;&nbsp;&nbsp;&nbsp;Set VariableNames = New Collection
&nbsp;&nbsp;&nbsp;&nbsp;Set Variables = New Collection
&nbsp;&nbsp;&nbsp;&nbsp;Me.Show
&nbsp;&nbsp;&nbsp;&nbsp;Host = InputBox("Server's host or IP address", , "localhost")
&nbsp;&nbsp;&nbsp;&nbsp;CS.Connect Host, STANDARD_PORT
&nbsp;&nbsp;&nbsp;&nbsp;User = InputBox("Your username", , "johndoe")
&nbsp;&nbsp;&nbsp;&nbsp;Password = InputBox("Your password", , "pollywog")
&nbsp;&nbsp;&nbsp;&nbsp;DoEvents
&nbsp;&nbsp;&nbsp;&nbsp;SendMessage CS, "LogIn", User, Password
&nbsp;&nbsp;&nbsp;&nbsp;DoEvents
End Sub
<BR><BR><FONT COLOR="#009900">'Unintentionally lost the connection</FONT>
Private Sub CS_Disconnect()
&nbsp;&nbsp;&nbsp;&nbsp;SetVar.Enabled = False
&nbsp;&nbsp;&nbsp;&nbsp;MsgBox "You've been disconnected :("
End Sub
<BR><BR><FONT COLOR="#009900">'Message data have arrived from the server</FONT>
Private Sub CS_DataArrival(Bytes As Long)
&nbsp;&nbsp;&nbsp;&nbsp;Dim Message, Buffer
&nbsp;&nbsp;&nbsp;&nbsp;Buffer = Buffer & CS.Receive
&nbsp;&nbsp;&nbsp;&nbsp;While ParseMessage(Buffer, Message)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ProcessMessage CS, Message
&nbsp;&nbsp;&nbsp;&nbsp;Wend
End Sub
<BR><BR><FONT COLOR="#009900">'The user clicked "Launch Another Client"</FONT>
Private Sub AnotherClient_Click()
&nbsp;&nbsp;&nbsp;&nbsp;Dim NewClient As New Client
&nbsp;&nbsp;&nbsp;&nbsp;NewClient.Show
End Sub
<BR><BR><FONT COLOR="#009900">'The user clicked "Set"</FONT>
Private Sub SetVar_Click()
&nbsp;&nbsp;&nbsp;&nbsp;SendMessage CS, "SetValue", _
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;VarName.Text, VarValue.Text
End Sub
</PRE>
</TD></TR></TABLE></CENTER>
<P>As with the server, the core of the client's operation is the <TT>ProcessMessage</TT> subroutine. Since both the
client and server use many of the same mechanisms, we'll be putting them into a shared library module we'll call
"Shared" (".bas"):
<P><CENTER><TABLE BGCOLOR="#FFFFDD" CELLSPACING="0" CELLPADDING="4" BORDER="1"><TR><TD>
<PRE>
<FONT COLOR="#009900">'The port the server listens for connections on</FONT>
Public Const STANDARD_PORT = 300
<BR><BR><FONT COLOR="#009900">'The start-up routine</FONT>
Public Sub Main()
&nbsp;&nbsp;&nbsp;&nbsp;Dim NewClient As New Client
&nbsp;&nbsp;&nbsp;&nbsp;If MsgBox("Want to launch a server?", vbYesNo) = vbYes Then
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server.Show
&nbsp;&nbsp;&nbsp;&nbsp;End If
&nbsp;&nbsp;&nbsp;&nbsp;NewClient.Show
End Sub
<BR><BR><FONT COLOR="#009900">'Set an item in the collection</FONT>
Public Sub SetItem(Col, Key, Value)
&nbsp;&nbsp;&nbsp;&nbsp;Dim Temp
&nbsp;&nbsp;&nbsp;&nbsp;On Error Resume Next
&nbsp;&nbsp;&nbsp;&nbsp;Temp = Col(Key)
&nbsp;&nbsp;&nbsp;&nbsp;If Err.Number = 0 Then Col.Remove Key
&nbsp;&nbsp;&nbsp;&nbsp;On Error GoTo 0
&nbsp;&nbsp;&nbsp;&nbsp;Col.Add Value, Key
End Sub
<BR><BR><FONT COLOR="#009900">'Replace "unsafe" characters with metacharacters</FONT>
Public Function Encode(Value)
&nbsp;&nbsp;&nbsp;&nbsp;Encode = Replace(Value, "\", "\b")
&nbsp;&nbsp;&nbsp;&nbsp;Encode = Replace(Encode, " ", "\s")
&nbsp;&nbsp;&nbsp;&nbsp;Encode = Replace(Encode, vbCr, "\c")
&nbsp;&nbsp;&nbsp;&nbsp;Encode = Replace(Encode, vbLf, "\l")
End Function
<BR><BR><FONT COLOR="#009900">'Replace metacharacters with their original characters</FONT>
Public Function Decode(Value)
&nbsp;&nbsp;&nbsp;&nbsp;Decode = Replace(Value, "\l", vbLf)
&nbsp;&nbsp;&nbsp;&nbsp;Decode = Replace(Decode, "\c", vbCr)
&nbsp;&nbsp;&nbsp;&nbsp;Decode = Replace(Decode, "\s", " ")
&nbsp;&nbsp;&nbsp;&nbsp;Decode = Replace(Decode, "\b", "\")
End Function
<BR><BR><FONT COLOR="#009900">'Encode and send a message</FONT>
Public Sub SendMessage(Socket, Name, ParamArray Parameters())
&nbsp;&nbsp;&nbsp;&nbsp;Dim Message, i
&nbsp;&nbsp;&nbsp;&nbsp;Message = Encode(Name)
&nbsp;&nbsp;&nbsp;&nbsp;For i = 0 To UBound(Parameters)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Message = Message & " " & _
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Encode(Parameters(i))
&nbsp;&nbsp;&nbsp;&nbsp;Next
&nbsp;&nbsp;&nbsp;&nbsp;Message = Message & vbCrLf
&nbsp;&nbsp;&nbsp;&nbsp;Socket.Send CStr(Message)
End Sub
<BR><BR><FONT COLOR="#009900">'Is there a complete message ready? Extract it and decode.</FONT>
Public Function ParseMessage(Buffer, Message)
&nbsp;&nbsp;&nbsp;&nbsp;Dim i
&nbsp;&nbsp;&nbsp;&nbsp;ParseMessage = False
&nbsp;&nbsp;&nbsp;&nbsp;i = InStr(1, Buffer, vbCrLf)
&nbsp;&nbsp;&nbsp;&nbsp;If i = 0 Then Exit Function
&nbsp;&nbsp;&nbsp;&nbsp;Message = Split(Left(Buffer, i - 1), " ")
&nbsp;&nbsp;&nbsp;&nbsp;Buffer = Mid(Buffer, i + 2)
&nbsp;&nbsp;&nbsp;&nbsp;For i = 0 To UBound(Message)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Message(i) = Decode(Message(i))
&nbsp;&nbsp;&nbsp;&nbsp;Next
&nbsp;&nbsp;&nbsp;&nbsp;ParseMessage = True
End Function
</PRE>
</TD></TR></TABLE></CENTER>
<P>Be sure to make "<TT>Sub Main</TT>" the start-up object in the project's properties.
<P><FONT SIZE="+1" COLOR="#0066FF"><B> Process Flow </B></FONT>
<BR>Now let's analyze what's going on here. First, since the server has to handle multiple sessions, it needs to
maintain session data for each session. This happens as soon as the connection is established in the
<TT>SSB_Connected()</TT> event handler. The ServerSocket object passed in, called "<TT>Socket</TT>", has its
<TT>ExtraTag</TT> value set to a new Collection object, which we'll use to hold session data for this
connection/session. We add three values to it: "LoggedIn", "User", and "Buffer". "LoggedIn" is a boolean value
indicating whether or not the client has properly logged in. We don't want the client to do anything else until
that happens. "User" is the ID of the user that logged in. "Buffer" is where we'll temporarily store all data
received from the client until we detect and parse out a complete message for processing.
<P>The <TT>ParseMessage()</TT> function in the shared module is called whenever data are received. This routine
looks for the first occurrence of <TT>&lt;CR&gt;&lt;LF&gt;</TT>, indicating the end of a complete message. If it
finds it, it grabs everything before this new-line, splits it up by space characters, and puts the parts into the
<TT>Message</TT> array. Naturally, it shortens the buffer to discard this message from it. <TT>ParseMessage()</TT>
returns true only if it does detect and parse one complete message. There could be more, but this function only
cares about the first one it finds.
<P>Once a message is found, <TT>ProcessMessage</TT> is called, with the array containing the parsed message passed
in. This routine will immediately exit if the client has not yet logged in, unless this message is actually the
"LogIn" command. Otherwise, The "<TT>Select Case Message(0)</TT>" block directs control to whatever block of code
is associated with <TT>Message(0)</TT>, the message name.
<P>Of course, the server needs to send messages to the client, too. It does this using the <TT>SendMessage()</TT>
subroutine in the shared library, which takes the message parts and encodes them into our message format, being sure
to translate "unsafe" characters like spaces into their metacharacter counterparts. It then sends this formatted
message to the indicated socket control.
<P>This is really all the server does. Of particular note, however, is what happens when a client sends the
"SetValue" command message. Not only does the server update its list of variables. It also broadcasts a message to
all the clients indicating that that value has changed using the <TT>.BroadCast()</TT> method of the
ServerSocketBank control.
<P>Now on to the client. The client form uses the same basic methodology, including the use of
<TT>ParseMessage()</TT>, and <TT>SendMessage()</TT>, and <TT>ProcessMessage()</TT> (which is different for the
client, of course, since it has to deal with different messages).
<P>Where the client really differs from the server is in its initialization sequence. Upon loading, the client
immediately tries to connect to the server (with the user providing details of where to find the server and whom to
log in as). As soon as it's connected, it sends the "LogIn" message with the provided user information.
<P>When the user clicks on the "Set" button, the client sends a "SetValue" message with the variable's name and
value. As was mentioned before, the server responds by broadcasting to all the connected clients the new value and
identifying which user changed it.
<P><FONT SIZE="+1" COLOR="#0066FF"><B> How can We Use this? </B></FONT>
<BR>Taking a step back, it seems rather silly to imagine that anyone would want to actually use our client / server
application the way it is. But it does demonstrate a powerful concept rarely employed in even the most modern
business applications: real-time refresh. What if, for example, a typical data entry form connected to a database
were automatically updated when another user changed some part of the data this user is looking at? This paradigm
is also used in all online chat systems. It can be used for shared blackboards or spreadsheets.
<P>The particularly neat thing about this approach to real-time refreshing is that the client is not expected to
occasionally poll the server for the latest stuff - which may be a total refresh of the relevant screen or data.
The server actively sends updated data to all the clients as information changes.
<P>If we wanted to be able to pass binary data, like files or images, we could make the <TT>ParseMessage()</TT>
routine a little more sophisticated by buffering bytes instead of string data (using the Sockets controls'
<TT>.ReceiveBinary()</TT> methods). The <TT>ProcessMessage</TT> routine could then turn the message name into text
and the individual message handlers could decide which parameters to translate into plain text and which to use in
binary form. (Be aware, though, that the buffers used by the Sockets controls can only store as much as any VB byte
array - about 32KB. One may need to send multiple messages if he needs to transmit a large chunk of binary data.)
<A NAME="conclusion">
<P><FONT SIZE="+2" COLOR="#000066"><B> Conclusion </B></FONT>
<BR>Programming Internet applications opens up a whole new vista of opportunities. This is especially true as
organizations are realizing that they no longer have to commit their budgets to single-platform solutions.
Integrating disparate systems using TCP/IP as a common communication protocol gives unprecedented flexibility. The
Sockets package provides an excellent way to quickly and painlessly build both client and server systems. These can
be the glue that binds together lots of existing systems both inside and outside a corporate intranet. Or they can
be used to develop complete end products from web browsers to database engines.
<P>The use of the Internet protocols will only grow in the coming years. It's not too late to jump on board. And
the simple truth is that there is no single Internet protocl - not HTTP, not MessageQ, nor any other - that yet
answers the needs of all applications. That's why people keep developing new ones. Starting at the essential
foundation - TCP/IP itself - ensures one the greatest flexibility of choices and can even help free one from the
dangers of proprietary standards that can lock one in to a single vendor and platform, like Microsoft's DCOM.
<P>Internet programming is power. The Sockets package makes it easy.

