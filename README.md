YasuhiroABE.mediawiki-restoredb
===============================
This module might be useful to setup another system using the copy of the existing mediawiki system or to restore the service from a db backup file.

Requirements
------------
This module expects that the target host is a LAMP stack.
If your mysql server is working on another host, such cases have not been tested.

It is tested on the following system with mediawiki LTS 1.39.3 and former LTS (1.35.9) db dump file.

* Ubuntu 22.04.2 LTS (amd64)

### Additional Files

At least, this module expects that following files are placed.

* files/LocalSettings.php.j2
* files/mediawiki-1.39.3.tar.gz
* files/dump.sql.gz

If you need extensions, these targzball should be placed as follows.

* files/__extension-name__.tar.gz

To copy another file, please check the *memi_mediawiki_additional_files* variable at the next section.

Role Variables
--------------

	## mediawiki extract directory
	memi_mediawiki_destdir: "/var/www/html/mediawiki"

	## mediawiki targzball filepath
	memi_mediawiki_filepath: "files/mediawiki-1.31.0.tar.gz"
	## list of extension targzball filepath which will extract on {{ memi_mediawiki_destdir }}/extensions/
	## e.g. - "files/GraphViz-REL1_31-9abad17.tar.gz"
	memi_mediawiki_extensions_filepath: []
	  
	## copy additional files
	## e.g. - { src: "toplogo.png", dest: "{{ memi_mediawiki_destdir }}/images" }
	memi_mediawiki_additional_files: []
	  
	####################
	## mysql settings ##
	####################
	## mysql dump filepath.
	memi_restoredb_filepath: "files/dump.sql.gz"
	##
	## * Please overwrite followings on playbook.yml
	##
	memi_restore_dbname: "wikidb"
	memi_mysql_user: "wikiuser"
	memi_mysql_pass: "secret"

Dependencies
------------

N/A

Example Playbook
----------------

    - hosts: all
	  vars:
	    memi_restore_dbname: "mwiki"
        memi_mysql_user: "wikiuser"
        memi_mysql_pass: "secret"
		memi_mediawiki_extensions_filepath:
          - "files/GraphViz-REL1_31-9abad17.tar.gz"
        memi_mediawiki_additional_files:
          - { src: "files/toplogo.png", dest: "{{ memi_mediawiki_destdir }}/images" }
      roles:
         - yasuhiroabe.mediawiki-migration

License
-------

Apache License 2.0

Author Information
------------------

[Yasuhiro ABE](http://www.yasundial.org/foaf.xml)

