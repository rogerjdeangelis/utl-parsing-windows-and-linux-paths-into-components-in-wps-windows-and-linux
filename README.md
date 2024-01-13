# utl-parsing-windows-and-linux-paths-into-components-in-wps-windows-and-linux
Parsing windows and linux paths into components in wps windows and linux  
    %let pgm=utl-parsing-windows-and-linux-paths-into-components-in-wps-windows-and-linux;

    Parsing windows and linux paths into components in wps windows and linux

    github
    http://tinyurl.com/w9frn4jh
    https://github.com/rogerjdeangelis/utl-parsing-windows-and-linux-paths-into-components-in-wps-windows-and-linux

    Very old code before the enhanced string functions in wps windows and linux;

    Works at macro time and datastep time

    This works best with vanilla paths without imbedded special characters.
    Should work in linux and windows?
    Not thoroughly tested bit should work?.

      PARSING MACROS                 macro

         1 getDrive                  getDrive(pth)/ des="Extract drive letter from the full path"
         2 getFileStem               getFileStem(pth)/des="Extract the path without the file name and extension"
         3 getFileWithoutExtension   getFileWithoutExtension(pth)/des="get file name without extension"
         4 getFileWithExtension      getFileWithExtension(pth) / des="Extract the file with extension from the full path"

    /*----                                                                   ----*/
    /*----   INPUT                                                           ----*/
    /*----                                                                   ----*/
    %let full_path =d:/groundtruth/production/utl/testing.sas;

    /*              _      _      _
    / |   __ _  ___| |_ __| |_ __(_)_   _____
    | |  / _` |/ _ \ __/ _` | `__| \ \ / / _ \
    | | | (_| |  __/ || (_| | |  | |\ V /  __/
    |_|  \__, |\___|\__\__,_|_|  |_| \_/ \___|
         |___/
    */

    /*----                                                                   ----*/
    /*----   EXTRACT DRIVE LETTER FROM THE FULL PATH                         ----*/
    /*----                                                                   ----*/

    filename FT15F001 "c:/oto/getDrive.sas"; /*---- send to autocall folder  ----*/
    parmcards4;
    %macro getDrive(pth)/ des="Extract drive letter from the full path" ;
        %qscan(&pth,1,":");
    %mend getDrive;
    ;;;;
    run;quit;

    %put  Drive = %getDrive(&full_path);


    /*----                                                                   ----*/
    /*----   OUTPUT                                                          ----*/
    /*----

    Drive = d

    /*___               _   _____ _ _      ____  _
    |___ \    __ _  ___| |_|  ___(_) | ___/ ___|| |_ ___ _ __ ___
      __) |  / _` |/ _ \ __| |_  | | |/ _ \___ \| __/ _ \ `_ ` _ \
     / __/  | (_| |  __/ |_|  _| | | |  __/___) | ||  __/ | | | | |
    |_____|  \__, |\___|\__|_|   |_|_|\___|____/ \__\___|_| |_| |_|
             |___/
    */

    /*----                                                                   ----*/
    /*----   EXTRACT DRIVE LETTER FROM THE FULL PATH                         ----*/
    /*----                                                                   ----*/

    filename FT15F001 "c:/oto/getFileStem.sas"; /*---- send to autocall      ----*/
    parmcards4;
    %macro getFileStem(pth)/des="Extract the path without the file name and extension";
      %local revstr cutstr gotstm;
      %let revstr=%qleft(%qsysfunc(reverse(&pth)));
      %let cutstr=%qsubstr(&revstr,%qsysfunc(indexc(&revstr,%str(/\))));
      %let gotstm=%qleft(%qsysfunc(reverse(&cutstr)));
         %str(&gotstm)
    %mend getFileStem;
    ;;;;
    run;quit;

    %put  Drive and folders = %getFileStem(&full_path);

    /*----                                                                   ----*/
    /*----   OUTPUT                                                          ----*/
    /*----

    Drive and folders = d:/groundtruth/production/utl/

    /*____             _   _____ _ _    __        ___ _   _                _   _____        _                 _
    |___ /   __ _  ___| |_|  ___(_) | __\ \      / (_) |_| |__   ___  _   _| |_| ____|__  _| |_ ___ _ __  ___(_) ___  _ __
      |_ \  / _` |/ _ \ __| |_  | | |/ _ \ \ /\ / /| | __| `_ \ / _ \| | | | __|  _|  \ \/ / __/ _ \ `_ \/ __| |/ _ \| `_ \
     ___) | |(_| |  __/ |_|  _| | | |  __/\ V  V / | | |_| | | | (_) | |_| | |_| |___  >  <| ||  __/ | | \__ \ | (_) | | | |
    |____/  \__, |\___|\__|_|   |_|_|\___| \_/\_/  |_|\__|_| |_|\___/ \__,_|\__|_____|/_/\_\\__\___|_| |_|___/_|\___/|_| |_|
            |___/
    */

    /*----                                                                   ----*/
    /*----   EXTRACT FILE WITHOUT EXTENSION                                  ----*/
    /*----                                                                   ----*/

    filename FT15F001 "c:/oto/getFileWithoutExtension.sas"; /*---- autocall  ----*/
    parmcards4;
    %macro getFileWithoutExtension(pth)/des="get file name without extension";
      %qscan(&pth,-2,'./\')
    %mend getFileWithoutExtension;
    ;;;;
    run;quit;

    %put File Without Extension = %getFileWithoutExtension(&full_path);

    /*----                                                                   ----*/
    /*----   OUTPUT                                                          ----*/
    /*----

    File Without Extension = testing

    /*          _   _____ _ _    __        ___ _   _     _____      _                  _
      __ _  ___| |_|  ___(_) | __\ \      / (_) |_| |__ | ____|_  _| |_ ___ _ __   ___(_) ___  _ __
     / _` |/ _ \ __| |_  | | |/ _ \ \ /\ / /| | __| `_ \|  _| \ \/ / __/ _ \ `_ \ / __| |/ _ \| `_ \
    | (_| |  __/ |_|  _| | | |  __/\ V  V / | | |_| | | | |___ >  <| ||  __/ | | |\__ \ | (_) | | | |
     \__, |\___|\__|_|   |_|_|\___| \_/\_/  |_|\__|_| |_|_____/_/\_\\__\___|_| |_||___/_|\___/|_| |_|
     |___/

    */

    filename FT15F001 "c:/oto/getFileWithExtension.sas"; /*---- autocall     ----*/
    parmcards4;
    %macro getFileWithExtension(pth) / des="Extract the file with extension from the full path";
      %local revpth gotfyl;
      %let revpth=%qleft(%qscan(%qsysfunc(reverse(&Pth)),1,%str(/\)));
      %let gotfyl=%qleft(%qsysfunc(reverse(&revpth)));
         %str(&gotfyl)
    %mend getfilewithextension;
    ;;;;
    run;quit;

    %put File With Extension = %getFileWithExtension(&full_path);

    /*----                                                                   ----*/
    /*----   OUTPUT                                                          ----*/
    /*----

    File With Extension = testing.sas

        /*          _   _____ _ _      _____      _                 _                               
      __ _  ___| |_|  ___(_) | ___| ____|_  _| |_ ___ _ __  ___(_) ___  _ __                    
     / _` |/ _ \ __| |_  | | |/ _ \  _| \ \/ / __/ _ \ `_ \/ __| |/ _ \| `_ \                   
    | (_| |  __/ |_|  _| | | |  __/ |___ >  <| ||  __/ | | \__ \ | (_) | | | |                  
     \__, |\___|\__|_|   |_|_|\___|_____/_/\_\\__\___|_| |_|___/_|\___/|_| |_|                  
     |___/                                                                                      
    */                                                                                          
                                                                                                
    filename FT15F001 "c:/oto/getFileWithExtension.sas"; /*---- autocall     ----*/             
    parmcards4;                                                                                 
    %macro getFileExtension(pth)/des="get file name without extension";                         
      %qscan(&pth,-1,'.')                                                                       
    %mend getFileExtension;                                                                     
    ;;;;                                                                                        
    run;quit;                                                                                   
                                                                                                
    %put FileExtension = %getFileExtension(&full_path);                                         
                                                                                                
    /*----                                                                   ----*/             
    /*----   OUTPUT                                                          ----*/             
    /*----                                                                                      
                                                                                                
    File With Extension = sas                                                                   


    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
