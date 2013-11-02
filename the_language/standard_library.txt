Content-Type: text/x-zim-wiki
Wiki-Format: zim 0.4
Creation-Date: 2013-11-02T10:50:39+01:00

====== standard library ======
Created Saturday 02 November 2013

===== FileUtils =====
'''
pry(main)> ls FileUtils
FileUtils.methods: 
'''

            ''cd              compare_stream  install   mv                       rm         ''
''  chdir           copy            link      options                  rm_f       ''
''  chmod           copy_entry      ln        options_of               rm_r       ''
''  chmod_R         copy_file       ln_s      private_module_function  rm_rf      ''
''  chown           copy_stream     ln_sf     pwd                      rmdir      ''
''  chown_R         cp              makedirs  remove                   rmtree     ''
''  cmp             cp_r            mkdir     remove_dir               safe_unlink''
''  collect_method  getwd           mkdir_p   remove_entry             symlink    ''
''  commands        have_option?    mkpath    remove_entry_secure      touch      ''
''  compare_file    identical?      move      remove_file              uptodate?''  


* File.expand_path(path - possibly relative, may start with ~, starting-point = '.')
