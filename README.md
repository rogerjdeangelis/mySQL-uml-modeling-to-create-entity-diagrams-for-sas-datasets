# utl-uml-entity-diagrammin-with-sas-and-mysql
mySQL uml modeling to create entity diagrams for sas datasets 

    mySQL uml modeling to create entity diagrams for sas datasets                                                                        
    Entity Diagram
    https://tinyurl.com/yxfafnwq                                                                                                      
    https://github.com/rogerjdeangelis/mySQL-uml-modeling-to-create-entity-diagrams-for-sas-datasets/blob/master/my_little_schema.pdf                                                                                                                                       
    github                                                                                                                               
    https://tinyurl.com/y6rj6z2b                                                                                                         
    https://github.com/rogerjdeangelis/mySQL-uml-modeling-to-create-entity-diagrams-for-sas-datasets                                     
                                                                                                                                         
    If you use formats the typing in mySQL is close to SAS formating                                                                     
                                                                                                                                         
                                                                                                                                         
      What you need to do                                                                                                                
                                                                                                                                         
          1. Install MySQL  https://dev.mysql.com/downloads/installer/                                                                   
             I chose the 373mb version  (this is what it downloaded)                                                                     
             mysql-installer-community-8.0.16.0                                                                                          
                                                                                                                                         
          2. I installed all products on my workstation                                                                                  
                                                                                                                                         
          3. I had to add this to my path  C:\Program Files\MySQL\MySQL Server 8.0\lib                                                   
             (you need Microsoft Visual C++ 2015 or later?)                                                                              
                                                                                                                                         
          4. Create a simple SAS schema of two related tables see input below                                                            
                                                                                                                                         
          5. Copy you schema into the 'world' schema that comes with MySql.                                                              
             The world schema has only two small tables.                                                                                 
             I used SAS Access to MySQL to load the sas datasets into mySQL                                                              
                                                                                                                                         
          6. Open MySql workbench                                                                                                        
                                                                                                                                         
             database > reverse engineer > password > click on world > next > execute                                                    
             You should see your two tables along with tables city and country.                                                          
             Delete city and country.                                                                                                    
                                                                                                                                         
             Click on text icon (looks like a letter in the left toolbar) release button                                                 
             Move to where you want the text box and click once. You                                                                     
             should see a small beige box, Double click on the box and                                                                   
             enter your "My Little Schema"                                                                                               
                                                                                                                                         
             Very intuitive from here.                                                                                                   
                                                                                                                                         
             Right click on the lb table and select edit lb                                                                              
             Click of Patient and date and set a nonnull primary key                                                                     
                                                                                                                                         
             Right click on the dm table and select edit dm                                                                              
             Click of Patient  set a nonnull primary key                                                                                 
                                                                                                                                         
             To set up the one to many relationships from lb                                                                             
             to dm First click on the 'many' table 'lb' then                                                                             
             click on the dm table                                                                                                       
                                                                                                                                         
             If you have defined your keys correctly then a                                                                              
             one to many clawfoot will appear.                                                                                           
                                                                                                                                         
             To create new SAS schemas in mySQL you have to use the reverse engineer option.                                             
                                                                                                                                         
     *_                   _                                                                                                              
                                                                                                                                         
     *_                   _                                                                                                              
    (_)_ __  _ __  _   _| |_                                                                                                             
    | | '_ \| '_ \| | | | __|                                                                                                            
    | | | | | |_) | |_| | |_                                                                                                             
    |_|_| |_| .__/ \__,_|\__|                                                                                                            
            |_|                                                                                                                          
    ;                                                                                                                                    
                                                                                                                                         
    I use this tool to create a little relational schema.                                                                                
    Two datasets are also on github                                                                                                      
                                                                                                                                         
    github                                                                                                                               
    https://tinyurl.com/y2dap3yj                                                                                                         
    https://github.com/rogerjdeangelis/utl-make-fake-relational-clinical-tables-demographics-lab-exposure-adverse-events                 
                                                                                                                                         
    %include "c:/oto/utl_fakepac.sas";                                                                                                   
                                                                                                                                         
    proc format;                                                                                                                         
      value trt2des                                                                                                                      
      1='Aspirin'                                                                                                                        
      2='Placebo';                                                                                                                       
    run;quit;                                                                                                                            
                                                                                                                                         
    /* CREATE DEMOGRAPHICS */                                                                                                            
    %initfake(dm,4322,200) ;                        /* output dataset name, seed and number of patients */                               
    %fakeint(trtcd,1,3) ;                           /* treatment codes 1 or 2 */                                                         
    trt=put(trtcd,trt2des.);                        /* treatment description*/                                                           
    %fakecont(wtkg,120,20) ;                        /* random normal mean and standard deviation of weight */                            
    %fakecat(gender,C,$6.,Male Female) ;            /* random uniform genders */                                                         
    %fakedate(birthdt,'01JAN1940'd,'01JAN1960'd) ;  /* random uniformly distributed dates from  JAN 2006 to JAN2008 */                   
    %fakeint(age,50,90);                            /* random uniform ages from 50 to 90 */                                              
    format birthdt date9.;                                                                                                               
    %makefake ;                                                                                                                          
                                                                                                                                         
    /* CREATE LAB */                                                                                                                     
    %initfake(lb,4322,10) ;                                                                                                              
    %fakedate(labdt,'01JAN2006'd,'01JAN2008'd) ;                                                                                         
    %fakecont(wbc,130,10) ;                                                                                                              
    %fakecont(rbc,206,15) ;                                                                                                              
    %fakecont(sgt,99,20) ;                                                                                                               
    %fakecont(co2,44,6) ;                                                                                                                
    %fakecont(hct,36,2) ;                                                                                                                
    %fakecont(hgb,12,1) ;                                                                                                                
    %makefake ;                                                                                                                          
                                                                                                                                         
    * cosmetics for mysql typing;                                                                                                        
    data lb;                                                                                                                             
     format patient 2.                                                                                                                   
            labdt   date9.                                                                                                               
            wbc rbc sgt co2 hct hgb 7.2;                                                                                                 
     set lb(obs=10);                                                                                                                     
    run;quit;                                                                                                                            
                                                                                                                                         
    data dm;                                                                                                                             
     retain patient trt trtcd birthdt gender age wtkg;                                                                                   
     format patient 2. age 3.                                                                                                            
            birthdt   date9.                                                                                                             
             wtkg 7.2;                                                                                                                   
     set dm(obs=10);                                                                                                                     
    run;quit;                                                                                                                            
                                                                                                                                         
    *            _               _                                                                                                       
      ___  _   _| |_ _ __  _   _| |_                                                                                                     
     / _ \| | | | __| '_ \| | | | __|                                                                                                    
    | (_) | |_| | |_| |_) | |_| | |_                                                                                                     
     \___/ \__,_|\__| .__/ \__,_|\__|                                                                                                    
                    |_|                                                                                                                  
    ;                                                                                                                                    
                                                                                                                                         
                                                                                                                                         
      *******************************                  *******************************                                                   
      * dm                          *                  * lb                          *                                                   
      *                             *                  *                             *                                                   
      *  p patient   tinyint(4)     *                  *  p patient  tinyint(4)      *                                                   
      *  - trt       varchar(7)     *                  *  p labdt    DATE            *                                                   
      *  - trtcd     tinyint(4)     *                  *  - wbc      decimal(6,2)    *                                                   
      *  - birthdt   date           * One to Many      *  - rbc      decimal(6,2)    *                                                   
      *  - gender    varchar(6)     *                  *  - sgt      decimal(6,2)    *                                                   
      *  - age       smallint(6)    *-|--------------/ *  - co2      decimal(6,2)    *                                                   
      *  - wtkg      decimal(6,2)   *                \ *  - hct      decimal(6,2)    *                                                   
      *                             *                  *  - hgb      decimal(6,2)    *                                                   
      *******************************                  *                             *                                                   
      * Idexes                      *                  * dm patient   tineint(4)     *  * doc for foreign key                            
      *                             *                  *                             *                                                   
      * p Primary                   *                  *******************************                                                   
      *                             *                  *                             *                                                   
      *******************************                  * p Primary                   *                                                   
                                                       *                             *                                                   
                                                       * fk_lb_dm_idx                *                                                   
                                                       *                             *                                                   
                                                       *******************************                                                   
                                                                                                                                         
    *                                                                                                                                    
     _ __  _ __ ___   ___ ___  ___ ___                                                                                                   
    | '_ \| '__/ _ \ / __/ _ \/ __/ __|                                                                                                  
    | |_) | | | (_) | (_|  __/\__ \__ \                                                                                                  
    | .__/|_|  \___/ \___\___||___/___/                                                                                                  
    |_|                                                                                                                                  
    ;                                                                                                                                    
                                                                                                                                         
    * load tables into mysql;                                                                                                            
                                                                                                                                         
    libname mysqllib mysql user=root password="xxxxxxx" database=world port=3306;                                                       
                                                                                                                                         
    /*                                                                                                                                   
    proc sql;drop table dm  ;                                                                                                            
    proc sql;drop table lb ;                                                                                                             
    run;quit;                                                                                                                            
    */                                                                                                                                   
                                                                                                                                         
    proc copy in=work out=mysqllib;                                                                                                      
    select dm lb;                                                                                                                        
    run;quit;                                                                                                                            
                                                                                                                                         
    libname mysqllib clear;                                                                                                              
    Open MySql workbench                                                                                                                 
                                                                                                                                         
    database > reverse engineer > password > click on world > next > execute                                                             
    You should see your two tables along with tables city and country.                                                                   
    Delete city and country.                                                                                                             
                                                                                                                                         
    Click on text icon (looks like a letter in the left toolbar) release button                                                          
    Move to where you want the text box and click once. You                                                                              
    should see a small beige box, Double click on the box and                                                                            
    enter your "My Little Schema"                                                                                                        
                                                                                                                                         
    Very intuitive from here.                                                                                                            
                                                                                                                                         
    Right click on the lb table and select edit lb                                                                                       
    Click of Patient and date and set a nonnull primary key                                                                              
                                                                                                                                         
    Right click on the dm table and select edit dm                                                                                       
    Click of Patient  set a nonnull primary key                                                                                          
                                                                                                                                         
    To set up the one to many relationships from lb                                                                                      
    to dm First click on the 'many' table 'lb' then                                                                                      
    click on the dm table                                                                                                                
                                                                                                                                         
    If you have defined your keys correctly then a                                                                                       
    one to many clawfoot will appear.                                                                                                    
                                                                                                                                         
    To create new SAS schemas in mySQL you have to use the reverse engineer option.                                                      
                                                                                                                                         
