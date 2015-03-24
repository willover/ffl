### Module Description ###
The gsv module implements an interface to the gtk-server. This server
makes it possible for an ANSish forth engine to show a gtk GUI. Due to the
fact that using this module requires an external tool, this module has an
environmental dependency. See [GTKserver](GTKserver.md) for how to use this module.

### Module Words ###
#### Low level, overrulable gtk-server connection words ####
**gsv+connect** ( c-addr u -- ior )
> Connect to the gtk-server via a fifo c-addr u
**gsv+call** ( c-addr1 u1 -- c-addr2 u2 )
> Call the gtk-server with command c-addr1 u1, resulting in response c-addr2 u2
**gsv+disconnect** ( -- ior )
> Disconnect from the gtk-server, the gtk-server is **NOT** closed
#### gtk-server connection words ####
**gsv+open** ( c-addr1 u1 c-addr2 u2 -- ior )
> Open the connection to the gtk-server by reading the configuration file c-addr1 u1 and connecting to c-addr2 u2
**gsv+close** ( -- ior )
> Close the connection to the gtk-server, the gtk-server is **NOT** closed
#### gtk-server event words ####
**gsv+server-connect** ( n1 c-addr1 u1 n2 -- c-addr2 u2 )
> Call gtk\_server\_connect with widgetid n2, signal c-addr1 u1 and description n2, returning the result c-addr2 u2
**gsv+server-connect-after** ( n1 c-addr1 u1 n2 -- c-addr2 u2 )
> Call gtk\_server\_connect\_after with widgetid n2, signal c-addr1 u1 and description n2, returning the result c-addr2 u2
### Examples ###
```
include ffl/gsv.fs

\ Open the connection to the gtk-server and load all definitions from the gtk-server.cfg file

s" gtk-server.cfg" s" ffl-fifo" gsv+open 0= [IF]


  : event>widget   ( c-addr u -- n = Convert the event string to a widget id )
    0. 2swap >number 2drop d>s
  ;


  0 value window                \ the event widgets
  0 value exit-button
  

  : gsv-example
    gtk_init                    \ Initialise toolkit
                                \ Create toplevel window 
    GTK_WINDOW_TOPLEVEL gtk_window_new to window
    
    s" This is a title" window gtk_window_set_title
    
                                \ Layout the widgets with a table
    1 30 30 gtk_table_new >r
    
    r@ window gtk_container_add
    
                                \ Add a label to the window
    s" Hello world" gtk_label_new >r
    
    7 3 29 1 r@ r'@ gtk_table_attach_defaults
    
    r> gtk_widget_show
    
                                \ Add the exit button to the window
    s" Exit" gtk_button_new_with_label to exit-button   
    
    27 23 28 20 exit-button r@ gtk_table_attach_defaults

    exit-button gtk_widget_show
    
    r>          gtk_widget_show   \ Table
    
    window      gtk_widget_show   \ Window
    
                                  \ Main loop
    BEGIN
      s" WAIT" gtk_server_callback
      event>widget CASE
        window      OF true ENDOF
        exit-button OF true ENDOF
        false swap
      ENDCASE
    UNTIL
    
    0 gtk_exit
  ;

  gsv-example
  
  gsv+close drop
    
[ELSE]
  .( No gtk-server fifo, is the gtk-server running ? ) cr
[THEN]


\ ==============================================================================
```

---

Generated by **ofcfrth-0.10.0**