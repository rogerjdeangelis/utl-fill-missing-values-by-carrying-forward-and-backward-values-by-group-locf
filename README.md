# utl-fill-missing-values-by-carrying-forward-and-backward-values-by-group-locf
Fill missing values by carrying forward and backward values by group locf
    Fill missing values by carrying forward and backward values by group locf                                                                
                                                                                                                                             
    github                                                                                                                                   
    https://tinyurl.com/yc5rsp6p                                                                                                             
    https://github.com/rogerjdeangelis/utl-fill-missing-values-by-carrying-forward-and-backward-values-by-group-locf                         
                                                                                                                                             
    There are many solutions on the SAS forum, but many of them do not                                                                       
    seem to handle this five cases? especially endpoints.                                                                                    
                                                                                                                                             
    Perhaps you are better off using R than SAS roll your own solutions?                                                                     
                                                                                                                                             
    SAS Forum                                                                                                                                
    https://tinyurl.com/y8hbne8f                                                                                                             
    https://communities.sas.com/t5/SAS-Programming/Fill-missing-in-specific-rows-by-groups/m-p/645782                                        
                                                                                                                                             
    StackOverflow                                                                                                                            
    https://tinyurl.com/ybrvy58d                                                                                                             
    https://stackoverflow.com/questions/40040834/replace-na-with-previous-or-next-value-by-group-using-dplyr                                 
                                                                                                                                             
    SAS Forum                                                                                                                                
    https://tinyurl.com/yckqbrez                                                                                                             
    https://communities.sas.com/t5/SAS-Programming/Last-observation-carried-to-previous-missing-and-the-missing/td-p/501891                  
                                                                                                                                             
    *_                   _                                                                                                                   
    (_)_ __  _ __  _   _| |_                                                                                                                 
    | | '_ \| '_ \| | | | __|                                                                                                                
    | | | | | |_) | |_| | |_                                                                                                                 
    |_|_| |_| .__/ \__,_|\__|                                                                                                                
            |_|                                                                                                                              
    ;                                                                                                                                        
                                                                                                                                             
    options validvarname=upcase;                                                                                                             
    libname sd1 "d:/sd1";                                                                                                                    
    data sd1.have;                                                                                                                           
       retain id;                                                                                                                            
       array x x1-x5;                                                                                                                        
       do id=1 to 4;                                                                                                                         
          do j=1 to 6;                                                                                                                       
             do over x;                                                                                                                      
                if uniform(6543)<.5 then x=.;                                                                                                
                else x=int(100*uniform(6543));                                                                                               
             end;                                                                                                                            
             output;                                                                                                                         
          end;                                                                                                                               
       end;                                                                                                                                  
       drop j;                                                                                                                               
    run;quit;                                                                                                                                
                                                                                                                                             
    SD1.HAVE total obs=24                                                                                                                    
                                                                                                                                             
     ID    X1    X2    X3    X4    X5      X5                                                                                                
                                                                                                                                             
      1     .     .     .     .     .      80                                                                                                
      1    71    80    80    75    80      80  * carried upward                                                                              
      1    69     .     3     .    61      61                                                                                                
      1     .     .    69    83    66      66  * carried downward                                                                            
      1    93     .     .    59     .      66                                                                                                
      1     .    58    97     .    55      55                                                                                                
                                                                                                                                             
      2     .     .    45     .     .      49                                                                                                
      2    92     .    15    69     .      49                                                                                                
      2    63    42     .     .    49      49                                                                                                
      2    15     .     .    37    91      91                                                                                                
      2     .     .     .     .    13      13                                                                                                
      2    75    58     .    79    86      86                                                                                                
                                                                                                                                             
      3    38     .    96     .     .      84                                                                                                
      3    66    71    42     .    84      84                                                                                                
      3    47     .     4     .     .      84                                                                                                
      3    40    55     .     .     .      84                                                                                                
      3    83    14    47     .    85      85                                                                                                
      3    56    20    37     .    16      16                                                                                                
                                                                                                                                             
      4     .     4    85     .    84      84                                                                                                
      4     .    61     .     .    50      50                                                                                                
      4     .     .     .    91     .      50                                                                                                
      4    86     .    12     .    70      70                                                                                                
      4    83     .     .     .    35      35                                                                                                
      4     .     5     9     3    68      68                                                                                                
                                                                                                                                             
    *            _               _                                                                                                           
      ___  _   _| |_ _ __  _   _| |_                                                                                                         
     / _ \| | | | __| '_ \| | | | __|                                                                                                        
    | (_) | |_| | |_| |_) | |_| | |_                                                                                                         
     \___/ \__,_|\__| .__/ \__,_|\__|                                                                                                        
                    |_|                                                                                                                      
    ;                                                                                                                                        
                                                                                                                                             
    Up to 40 obs from WANT_R total obs=24                                                                                                    
                                                                                                                                             
     ID    X1    X2    X3    X4    X5                                                                                                        
                                                                                                                                             
      1    71    80    80    75    80                                                                                                        
      1    71    80    80    75    80                                                                                                        
      1    69    80     3    75    61                                                                                                        
      1    69    80    69    83    66                                                                                                        
      1    93    80    69    59    66                                                                                                        
      1    93    58    97    59    55                                                                                                        
      2    92    42    45    69    49                                                                                                        
      2    92    42    15    69    49                                                                                                        
      2    63    42    15    69    49                                                                                                        
      2    15    42    15    37    91                                                                                                        
      2    15    42    15    37    13                                                                                                        
      2    75    58    15    79    86                                                                                                        
      3    38    71    96     .    84                                                                                                        
      3    66    71    42     .    84                                                                                                        
      3    47    71     4     .    84                                                                                                        
      3    40    55     4     .    84                                                                                                        
      3    83    14    47     .    85                                                                                                        
      3    56    20    37     .    16                                                                                                        
      4    86     4    85    91    84                                                                                                        
      4    86    61    85    91    50                                                                                                        
      4    86    61    85    91    50                                                                                                        
      4    86    61    12    91    70                                                                                                        
      4    83    61    12    91    35                                                                                                        
      4    83     5     9     3    68                                                                                                        
                                                                                                                                             
                                                                                                                                             
    *                                                                                                                                        
     _ __  _ __ ___   ___ ___  ___ ___                                                                                                       
    | '_ \| '__/ _ \ / __/ _ \/ __/ __|                                                                                                      
    | |_) | | | (_) | (_|  __/\__ \__ \                                                                                                      
    | .__/|_|  \___/ \___\___||___/___/                                                                                                      
    |_|                                                                                                                                      
    ;                                                                                                                                        
                                                                                                                                             
    proc datasets lib=work nolist;                                                                                                           
      delete want_r;                                                                                                                         
    run;quit;                                                                                                                                
                                                                                                                                             
    %utlfkil(d:/xpt/want.xpt);                                                                                                               
                                                                                                                                             
    %utl_submit_r64('                                                                                                                        
    library(haven);                                                                                                                          
    library(tidyverse);                                                                                                                      
    library(SASxport);                                                                                                                       
    have<-read_sas("d:/sd1/have.sas7bdat");                                                                                                  
    have %>%                                                                                                                                 
      group_by(ID) %>%                                                                                                                       
      fill(X1, X2, X3, X4, X5, .direction = "down") %>%                                                                                      
      fill(X1, X2, X3, X4, X5, .direction = "up") -> want;                                                                                   
    write.xport(want,file="d:/xpt/want.xpt");                                                                                                
    ');                                                                                                                                      
                                                                                                                                             
    libname xpt xport "d:/xpt/want.xpt";                                                                                                     
    data want_r;                                                                                                                             
      set xpt.want;                                                                                                                          
    run;quit;                                                                                                                                
    libname xpt clear;                                                                                                                       
                                                                                                                                             
                                                                                                                                             
                                                                                                                                             
