# Fixed bug in screenshot program **Shooter** for OS Linux Mint 18.2 Sonya

If you try to upload file via FTP, but the link to the website is not copied to the clipboard, copy file `shutter` to the folder `/usr/bin/` with replacement.

#### CLI output error:
`GLib-CRITICAL **: Source ID 146 was not found when attempting to remove it at /usr/bin/shutter line 7249.
 *** unhandled exception in callback:
 *** Can't call method "append_file_name" on an undefined value at /usr/bin/shutter line 10742.
 *** ignoring at /usr/bin/shutter line 2884.`
 
#### Bug Fix:
1. Open terminal and paste

	`wget --no-check-certificate https://raw.githubusercontent.com/Varrcan/shutter-linuxmint-fix/master/shutter && sudo mv shutter /usr/bin/shutter`
	
2. Enter root password
3. Done!



#### Or manual Fix:
1. Open file as root `/usr/bin/shutter`
2. Find line #10737 - 10746
	```perl
	#copy website url to clipboard
	if ( $ftp_wurl_entry_dlg->get_text ) {
	    my $wuri = Gnome2::VFS::URI->new( $ftp_wurl_entry_dlg->get_text );
	
	    my ( $short, $folder, $ext ) = fileparse( $file, qr/\.[^.]*/ );
	    $wuri = $wuri->append_file_name( $short . $ext );
	    $clipboard->set_text( $wuri->to_string );
	    print "copied URI ", $wuri->to_string, " to clipboard\n"
	      if $sc->get_debug;
	}
	```
3. And replacement to:
	```perl
	#copy website url to clipboard
	if ( $ftp_wurl_entry_dlg->get_text ) {
	    my $wuri = $ftp_wurl_entry_dlg->get_text;
	
	    my ( $short, $folder, $ext ) = fileparse( $file, qr/\.[^.]*/ );
	    $wuri = $wuri . $short . $ext;
	    $clipboard->set_text( $wuri );
	    print "copied URI ", $wuri->to_string, " to clipboard\n"
	      if $sc->get_debug;
	}
	```
	
4. Done!
