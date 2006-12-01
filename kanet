#!/bin/sh
#  kaNet: network manager for Kanotix
#  hrb 26.02.06 version: kanet-0.02-01
#  start the tcl shell \
exec wish "$0" "$@" "$HOME"
#
#**************************************************************************************
#  proc.doToggle {} toggles the conn-flag and executes the 'on/off'-command
#**************************************************************************************
proc doToggle { } {
    global tog ic0 pom0 on1 on2 on3 on4 on5 of1 of2 of3 of4 of5
    global oncmd1 oncmd2 oncmd3 oncmd4 oncmd5 ofcmd1 ofcmd2 ofcmd3 ofcmd4 ofcmd5
#
    for { set cnt 1 } { $cnt <= 5 } { incr cnt } {
#      if { [expr \$$tog$cnt] } { puts "tog=*$tog*,cnt=$cnt; [expr \$${tog}cmd$cnt]" }
       if { [expr \$$tog$cnt] } {
         set cmd [expr \$${tog}cmd$cnt] ; set err [ catch { eval exec $cmd  } info ]
         if { $err } { tk_messageBox -type ok -message $info -parent . }
       } ;# if action required for this cnt
    } ;# for cnt = 1..5
    if { $tog == "on" } {
             set tog of ; $ic0 configure -bg green  ; $pom0 configure -bg green
    } else { set tog on ; $ic0 configure -bg yellow ; $pom0 configure -bg yellow }
  } ;#  end proc doToggle
#
#**************************************************************************************
#  proc.doCompress {} saves the xy-position, removes the big window and shows the icon
#**************************************************************************************
proc doCompress { } {
    global ic big bpos ipos
    set bgeo [ winfo geometry $big ] ; set igeo [ winfo geometry $ic ]
    regsub -all {x} $bgeo " " bgeox ; regsub -all {\+} $bgeox " " bgeol
    regsub -all {x} $igeo " " igeox ; regsub -all {\+} $igeox " " igeol
    set bw [lindex $bgeol 0] ; set bh [lindex $bgeol 1]
    set iw [lindex $igeol 0] ; set ih [lindex $igeol 1]
    if { $iw <= 1 } { set iw 45 } ; if { $ih <= 1 } { set ih 24 }
#
    set bx [expr [lindex $bgeol 2] -4] ; set by [expr [lindex $bgeol 3] - 24]
    set ix [expr $bx + $bw - $iw] ;  set iy [expr $by + $bh - $ih + 24]
    set bpos "+$bx+$by" ; set ipos "+$ix+$iy" ; saveConf
#puts "§kan36; bgeol= *$bgeol*, igeol= *$igeol*"
#puts "§kan37; ix,iy= *$ix*$iy, bpos= *$bpos*, ipos= *$ipos*"
    wm geometry $big $bpos ; wm geometry $ic $ipos ;# position both windows
    wm deiconify $ic ; wm withdraw $big ; update idletasks ;# remove big, show small
  } ;#  end proc doCompress
#
#**************************************************************************************
#  proc.doExpand displays the big window and iconifies the icon window
#**************************************************************************************
proc doExpand { } {
    global ic big
    wm deiconify $big ; wm withdraw $ic ; update idletasks
  } ;# proc.doExpand
#
#**************************************************************************************
#  proc.dohelp displays the help text
#**************************************************************************************
proc dohelp { } {
    global bpos wh ms7 ms21 ms22 ms23 ms24 ms25 ms26 ms27 ms28 ms29 ms30 ms31 ms32 ms33
    set wh .hlp  ; if { [winfo exists $wh ] } { destroy $wh }
    toplevel $wh ; wm geometry $wh $bpos ;  wm title $wh $ms7
    set wt $wh.t1 ; labelframe $wt
    label $wt.m21  -textvariable ms21  ; label $wt.m22  -textvariable ms22
    label $wt.m23  -textvariable ms23  ; label $wt.m24  -textvariable ms24
    label $wt.m25  -textvariable ms25  ; label $wt.m26  -textvariable ms26
    label $wt.m27  -textvariable ms27  ; label $wt.m28  -textvariable ms28
    label $wt.m29  -textvariable ms29  ; label $wt.m30  -textvariable ms30
    label $wt.m31  -textvariable ms31  ; label $wt.m32  -textvariable ms32
    label $wt.m33  -textvariable ms33
    set whb $wh.f2 ; labelframe $whb
    button $whb.b1 -textvariable ms4  -width 9 -command "destroy $wh"
    pack $wt.m21 $wt.m22 $wt.m23 $wt.m24 $wt.m25 $wt.m26 $wt.m27 $wt.m28 $wt.m29 \
      $wt.m30 $wt.m31 $wt.m32 $wt.m33 -anchor w
    pack $whb.b1 ; pack $wt $whb
  } ;# proc.dohelp
#
#**************************************************************************************
#  proc.doLang displays the messages: englisch / deutsch
#**************************************************************************************
proc doLang { } {
    global ms1 ms2 ms3 ms4 ms5 ms6 ms7 ms8 ms9 ms10 ms11 ms12 ms13 ms14 ms15 ms16
    global ms17 ms18 ms19 ms20 ms21 ms22 ms23 ms24 ms25 ms26 ms27 ms28 ms29 ms30 ms31
    global ms32 ms33 lang msen msde tx2
#
    update ; set ms14old $ms14 ;# save old text for user-defined cmd
    if { $lang == {en} } { set cnt 1 ; foreach ms $msen { set ms$cnt $ms ; incr cnt }
    }    else            { set cnt 1 ; foreach ms $msde { set ms$cnt $ms ; incr cnt } }
    if { $tx2 == $ms14old } { set tx2 $ms14 } ;# no manual entry
  } ;# proc.doLang
#
#**************************************************************************************
#  proc.saveConf {} writes the config-file kanet.conf
#**************************************************************************************
proc saveConf {} {
    global kacon pVersion bpos ipos initJobs initCompress lang
    global on1 on2 on3 on4 on5 of1 of2 of3 of4 of5 oncmd2 ofcmd2 tx2
    update ; if { ![catch { open $kacon w } fil ] } {
      catch { puts  $fil "confVersion    = $pVersion" }
      catch { puts  $fil "xy pos-big     = $bpos" }
      catch { puts  $fil "xy pos-icon    = $ipos" }
      catch { puts  $fil "connect @start = $initJobs" }
      catch { puts  $fil "compress@start = $initCompress" }
      catch { puts  $fil "language       = $lang" }
      catch { puts  $fil "on-1           = $on1" }
      catch { puts  $fil "on-2           = $on2" }
      catch { puts  $fil "on-3           = $on3" }
      catch { puts  $fil "on-4           = $on4" }
      catch { puts  $fil "on-5           = $on5" }
      catch { puts  $fil "of-1           = $of1" }
      catch { puts  $fil "of-2           = $of2" }
      catch { puts  $fil "of-3           = $of3" }
      catch { puts  $fil "of-4           = $of4" }
      catch { puts  $fil "of-5           = $on5" }
      catch { puts  $fil "oncommand2     = $oncmd2" }
      catch { puts  $fil "ofcommand2     = $ofcmd2" }
      catch { puts  $fil "text2          = $tx2" }
      catch { close $fil } } ;# end if open ok
  } ;# end proc. saveConfig
#
#**************************************************************************************
#  proc.doUserCmd does the dialog for the user-defined command
#**************************************************************************************
proc doUserCmd { } {
    global oncmd2 ofcmd2 tx2 ms14 ms15 ms16 ms17 ms18 ms19
#
    set udcmd .udcmd ; if { [winfo exists $udcmd ] } { return }
    toplevel $udcmd ; wm title $udcmd $ms15
#
    set udh $udcmd.fh ; frame $udh
    label $udh.m1 -textvariable ms16 ;  label $udh.m2 -textvariable ms17
    label $udh.m3 -textvariable ms18 ;  label $udh.m4 -textvariable ms19
    pack  $udh.m1 $udh.m2 $udh.m3 $udh.m4
#
    set uda $udcmd.fa ; labelframe $uda
    set uda1 $uda.f1 ; frame $uda1
    label $uda1.m1 -text "on-cmd" ; label $uda1.m2 -text "of-cmd"
    label $uda1.m3 -text "tx2"
    pack  $uda1.m1 $uda1.m2 $uda1.m3
#
    set uda2 $uda.f2 ; frame $uda2
    entry $uda2.e1 -width 30 -relief sunken -bd 2 -textvariable oncmd2
    entry $uda2.e2 -width 30 -relief sunken -bd 2 -textvariable ofcmd2
    entry $uda2.e3 -width 30 -relief sunken -bd 2 -textvariable tx2
    pack  $uda2.e1 $uda2.e2 $uda2.e3
    pack $uda1 $uda2 -side left
#
    set ubut $udcmd.but ; labelframe $ubut
    button $ubut.b1 -text "OK" -width 9 -command "saveConf ; destroy $udcmd"
    pack $ubut.b1
    pack $udh $uda $ubut -pady 16
  } ;# proc.doUserCmd
#
#**************************************************************************************
#  Main window: initialize variables
#**************************************************************************************
#
    set pVersion "kanet-0.02-01" ; set cVersion "kanet-unknown" ;# prog+conf versions
    set bpos +787+557 ; set ipos +928+693
    set dkacon "$argv/.kanet" ; set kacon $dkacon/kanetb.conf
#  initialize checkbuttons for the kanet actions: on/off, or connect/disconnect
    set on1  1 ;  set of1  1
    set on2  0 ;  set of2  0
    set on3  0 ;  set of3  0 ;  set tx3 "kMail"
    set on4  0 ;  set of4  0 ;  set tx4 "Konqueror"
    set on5  0 ;  set of5  0 ;  set tx5 "Firefox"
#
    set oncmd1 "pon dsl-provider"     ; set ofcmd1 "poff -a"
    set oncmd2 "pon dsl-provider"     ; set ofcmd2 "poff -a"
    set oncmd3 "/usr/bin/kmail &"     ; set ofcmd3 "killall -9 kmail"
    set oncmd4 "/usr/bin/konqueror &" ; set ofcmd4 "killall -9 konqueror"
    set oncmd5 "/usr/bin/firefox &"   ; set ofcmd5 "killall -9 firefox-bin"
#
#**************************************************************************************
    set msen [list "connect-actions" "compress" "Compress" "Exit" "userCmd" \
      "Help" "$pVersion help text" "language" "On program start" "on" \
      "- action on mouse click - " "off" "connect / disconnect" "(user-defined cmd)" \
      "$pVersion User-def.cmd" "Pls.click on entry field + type:" \
      "on-cmd = executed on 'connect'," \
      "off-cmd = executed on 'disconnect'," "txt = displayed in dialog field." \
      " " "kaNet supports convenient connection to the net." \
      "After a click on the coloured kaNet button, kaNet executes the selected" \
      "activities, and the button changes its colour:" \
      " - yellow -> green: 'connect'-activities," \
      " - green -> yellow: 'disconnect'-activities." \
      "The predefined programs (kMail etc.) can be started or stopped." \
      "The user can enter a connect/disconnect instruction at his/her own choice." \
      " " "A right-click on the kaNet-button compresses the window to that button,"\
      "another right-click restores the extended window." \
      "The kaNet window can be positioned on the desktop by drag 'n drop," \
      "and will re-assume its display mode after re-start or re-boot," \
    "provided that the window has been compressed at least once in the new position." ]
#
#**************************************************************************************
    set msde [list "Einwahl-Aktionen" "Einklappen" "Einklappen" "Beenden" \
    "Eig.Befehl"  "Hilfe" "$pVersion Hilfe-Text" "Sprache" "Bei Programmstart" "ein" \
    "- Aktion bei Mausklick - " "aus" "Einwahl / Abwahl"  "(eigener Befehl)" \
    "$pVersion Eig.Befehl" "Bitte Eingabefeld anklicken + eingeben:" \
    "on-cmd = wird bei 'Einwahl' ausgeführt," \
    "off-cmd = wird bei 'Abwahl' ausgeführt," "txt = wird im Dialogfeld angezeigt." \
    " " "kaNet unterstützt die bequeme Einwahl ins Netz." \
    "Nach einem Mausklick auf den farbigen kaNet-Knopf führt kaNet die angewählten" \
    "Aktionen aus, und der Knopf ändert seine Farbe:" \
    " - gelb -> grün: Einwahl-Aktionen," " - grün -> gelb: Abwahl-Aktionen." \
    "Die vorbelegten Programme (kMail etc) können gestartet bzw. beendet werden." \
    "Der Benutzer kann einen Einwahl/Abwahl-Befehl eigener Wahl eingeben." " " \
    "Rechts-Klick auf den kaNet-Knopf klappt das kaNet-Fenster ein," \
    "ein zweiter Rechts-Klick klappt es wieder aus." \
    "Das kaNet-Fenster lässt sich auf dem Bildschirm mit gedrückter Maustaste" \
    "positionieren und behält nach Neustart bzw. Systemstart seine Position," \
    "sofern das Fenster an der neuen Position mindestens einmal eingeklappt wurde." ]
#**************************************************************************************
#
    set initJobs 0 ; set initCompress 0 ; set tog on
    set lang en    ; set ms14 [ lindex $msen 13 ] ; set tx2 $ms14
#
#  read config file, if available
#
    if { ![file isdirectory $dkacon] } { catch [exec mkdir -p $dkacon] }
    if { ![catch {open $kacon} fil] } {
      while {1} { gets $fil curl ; if { [eof $fil] } { break } ; lappend lconf $curl }
#     read VersionCode from kanet.conf; if version ok: read kanet.conf completely
      catch { set cVersion [string trim [string range [lindex $lconf 0] 17 end ]] }
      if { [string range $cVersion 0 9] == [string range $pVersion 0 9] } {
        set bpos  [string trim [string range [lindex $lconf 1] 17 end ]]
        set ipos  [string trim [string range [lindex $lconf 2] 17 end ]]
        set initJobs     [string trim [string range [lindex $lconf 3] 17 end ]]
        set initCompress [string trim [string range [lindex $lconf 4] 17 end ]]
        set lang  [string trim [string range [lindex $lconf 5] 17 end ]]
#
        set on1  [string trim [string range [lindex $lconf 6] 17 end ]]
        set on2  [string trim [string range [lindex $lconf 7] 17 end ]]
        set on3  [string trim [string range [lindex $lconf 8] 17 end ]]
        set on4  [string trim [string range [lindex $lconf 9] 17 end ]]
        set on5  [string trim [string range [lindex $lconf 10] 17 end ]]
        set of1  [string trim [string range [lindex $lconf 11] 17 end ]]
        set of2  [string trim [string range [lindex $lconf 12] 17 end ]]
        set of3  [string trim [string range [lindex $lconf 13] 17 end ]]
        set of4  [string trim [string range [lindex $lconf 14] 17 end ]]
        set of5  [string trim [string range [lindex $lconf 15] 17 end ]]
        set oncmd2  [string trim [string range [lindex $lconf 16] 17 end ]]
        set ofcmd2  [string trim [string range [lindex $lconf 17] 17 end ]]
        set tx2     [string trim [string range [lindex $lconf 18] 17 end ]]
      } ; # end if version ok
    } ;# if config file ok
#
#**************************************************************************************
#  Main window: configure + position
#**************************************************************************************
#
    set big . ;  wm title $big $pVersion ; wm geometry $big $bpos ; doLang
    set biga .biga ; labelframe $biga
    set conf $biga.conf ; labelframe $conf
#
#   Job list: checkbuttons for actions at 'on' and 'off'
#
    set fj $conf.fj ; labelframe $fj
    set fj0 $fj.f0 ; labelframe $fj0 ;  label $fj0.m2 -textvariable ms11
    label $fj0.m1 -textvariable ms10 ;  label $fj0.m3 -textvariable ms12
    pack $fj0.m1 -side left -anchor w ; pack $fj0.m3 -side right -anchor e
    pack $fj0.m2 -side left -anchor c
#
    set fj1 $fj.f1 ; labelframe $fj1
# left: checkbuttons for 'on' actions
    set fja $fj1.fa ; frame $fja
    checkbutton $fja.b1 -variable on1 -command "saveConf"
    checkbutton $fja.b2 -variable on2 -command "saveConf"
    checkbutton $fja.b3 -variable on3 -command "saveConf"
    checkbutton $fja.b4 -variable on4 -command "saveConf"
    checkbutton $fja.b5 -variable on5 -command "saveConf"
    pack  $fja.b1 $fja.b2 $fja.b3 $fja.b4 $fja.b5 -anchor w
# center: labels
    set fjb $fj1.fb ; frame $fjb
    label $fjb.m1 -textvariable ms13
    label $fjb.m2 -textvariable tx2 ; label $fjb.m3 -textvariable tx3
    label $fjb.m4 -textvariable tx4 ; label $fjb.m5 -textvariable tx5
    pack  $fjb.m1 $fjb.m2 $fjb.m3 $fjb.m4 $fjb.m5 -expand 1 -fill x
# right: checkbuttons for 'off' actions
    set fjc $fj1.fc ; frame $fjc
    checkbutton $fjc.b1 -variable of1 -command "saveConf"
    checkbutton $fjc.b2 -variable of2 -command "saveConf"
    checkbutton $fjc.b3 -variable of3 -command "saveConf"
    checkbutton $fjc.b4 -variable of4 -command "saveConf"
    checkbutton $fjc.b5 -variable of5 -command "saveConf"
    pack  $fjc.b1 $fjc.b2 $fjc.b3 $fjc.b4 $fjc.b5 -anchor w
    pack $fja -side left ; pack $fjc -side right ; pack $fjb -expand 1 -fill x
    pack $fj0 $fj1 -side top -expand 1 -fill x
#
#  checkbuttons for actions on program start
    set fs $conf.fs ; labelframe $fs -text $ms9 ;# checkbutton part of big window
    checkbutton $fs.b1 -textvariable ms1 -variable initJobs
    checkbutton $fs.b2 -textvariable ms2 -variable initCompress
    pack  $fs.b1 $fs.b2 -anchor w
#
#  checkbuttons for 'language'
    set flan $conf.flan ; labelframe $flan -text $ms8
    radiobutton $flan.r1 -text "english" -variable lang -value en -command doLang
    radiobutton $flan.r2 -text "deutsch" -variable lang -value de -command doLang
    pack $flan.r1 $flan.r2 -side right
#
#  buttons
    set but $conf.but ; labelframe $but ;# button part of big window
    set but1 $but.f1 ; frame $but1 ; set but2 $but.f2 ; frame $but2
    button $but1.b0 -textvariable ms3 -width 9 -command "doCompress ; saveConf"
    button $but1.b1 -textvariable ms4 -width 9 -command "saveConf ; destroy . ; return"
    button $but2.b0 -textvariable ms5 -width 9 -command "doUserCmd"
    button $but2.b1 -textvariable ms6 -width 9 -command "dohelp"
    pack  $but1.b1 $but1.b0 -side right ; pack  $but2.b1 $but2.b0 -side right
    pack $but1 $but2 -side bottom
#  pack the expanded frame:
    pack $but -side bottom ; pack $flan $fs $fj -side bottom -padx 18 -pady 6
#
#  build the kaNet-button
    set pom $biga.pom ; labelframe $pom
    set pom0 $pom.m0 ; label $pom0 -text "kaNet" -bg yellow ; pack $pom0
    bind $pom0 <Button-1> doToggle ;  bind $pom0 <Button-3> doCompress
    pack  $pom $conf -side bottom -anchor e
    pack $biga -side right
#
#  small mini-window (icon-like) for pon/poff
    set ic .ic ; toplevel $ic ; wm overrideredirect $ic 1 ; wm withdraw $ic
    wm geometry $ic $ipos ; set icf $ic.f0 ; labelframe $icf
    set ic0 $icf.m0 ; label $ic0 -text "kaNet" -bg yellow ; pack $ic0 ; pack $icf
    bind $ic0 <Button-1> doToggle ;  bind $ic0 <Button-3> doExpand
#
    if { $initJobs } { doToggle } ;# connect/compress on start
    if { $initCompress } { wm deiconify $ic ; wm withdraw $big ; update idletasks }
#################        ** end proc.kaNet **        ##################################
#**************************************************************************************
