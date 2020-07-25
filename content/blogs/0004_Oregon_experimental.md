Template for .Rmd
================

For Oregon Do once

``` r
## TODO - fs:: to check if .csv files exist

# a few secs
if file_ (!here(data, "or_ppp.csv")) {
    loans  <- readr::read_csv(here("data-raw","foia_150k_plus.csv.zip")) %>% 
        filter(State == "OR") %>%
        write_csv(here("data","or_ppp.csv"))
}
```

``` r
ppp <- loans
# glimpse(loans)

or_ppp   <- loans %>% 
  arrange(State, DateApproved)
#or_ppp

or_ppp %>% 
  distinct(LoanRange)
      ## # A tibble: 5 x 1
      ##   LoanRange           
      ##   <chr>               
      ## 1 b $2-5 million      
      ## 2 c $1-2 million      
      ## 3 d $350,000-1 million
      ## 4 e $150,000-350,000  
      ## 5 a $5-10 million
```

``` r
# distinct lenders, 289
or_lenders  <- or_ppp %>% distinct(Lender)

# or_ppp %>% group_by(Lender) %>% count(sort=TRUE)

lender_jobs_retained  <- or_ppp %>% 
    group_by(Lender) %>% 
    summarize( total_loans = n(), total_jobs = sum(JobsRetained, rm.NA=TRUE) ) %>% 
    arrange(desc(total_jobs))

# total loans by DateApproved
# glimpse(or_ppp)
g  <- or_ppp %>% 
    group_by(DateApproved) %>% 
    summarize( total_loans = n()) %>% 
    ggplot(aes(x=DateApproved, y=total_loans)) +
    geom_point()


# tualatin, 171 companies
res  <- or_ppp %>% 
    filter(City=="TUALATIN") %>% 
    select(BusinessName)

print(res, n=200)
      ## # A tibble: 171 x 1
      ##     BusinessName                                
      ##     <chr>                                       
      ##   1 CARCO INDSUTRIES HOLDING LLC                
      ##   2 POWDER TECH, INC                            
      ##   3 COLUMBIA STONE, INC.                        
      ##   4 ARNETT INDUSTRIES LLC                       
      ##   5 WESTERN PRECISION PRODUCTS, INC             
      ##   6 HEWN ELEMENTS LLC                           
      ##   7 WAVE-FORM SYSTEMS, INCORPORATE              
      ##   8 WSM MANUFACTURING LLC                       
      ##   9 HAUGE PROVISIONS OF OREGON INC.             
      ##  10 SPINAL DIAGNOSTICS, PC                      
      ##  11 TUALATIN IMAGING, PC                        
      ##  12 WILDWOOD TRADING GROUP, INC.                
      ##  13 POWIN ENERGY CORPORATION                    
      ##  14 CC OF OREGON                                
      ##  15 THERMAL MODIFICATION TECHNOLOGIES INC.      
      ##  16 OR KREW, LLC                                
      ##  17 IGRAFX, LLC                                 
      ##  18 NW4S, INC.                                  
      ##  19 VERSUS 11, LLC                              
      ##  20 CEDAR LANDSCAPE CONSTRUCTION, INC.          
      ##  21 KLER SUN LLC                                
      ##  22 NATURAL STONE DESIGNS, INC.                 
      ##  23 INTEGRATED METAL COMPONENTS INC.            
      ##  24 J2D LLC                                     
      ##  25 CENTRO CHIROPRACTIC CLINIC OF OREGON, LLC   
      ##  26 SLATER & ASSOCIATES INSURANCE, INC.         
      ##  27 WILLAMETTE VALLEY ANIMAL HOSPITAL OF TUALAT…
      ##  28 GB II CORPORATION                           
      ##  29 PROFILE DOCUMENT SERVICES, INC              
      ##  30 ASCENTEC ENGINEERING LLC                    
      ##  31 PACIFIC MOTION, L.L.C.                      
      ##  32 EMERICK CONSTRUCTION COMPANY                
      ##  33 SUBURBAN DOOR CO.                           
      ##  34 ANODIZE SOLUTIONS LLC                       
      ##  35 BARHYTE SPECIALTY FOODS, INC.               
      ##  36 FUNTIME R.V., INC.                          
      ##  37 LINESCAPE LLC                               
      ##  38 LINESCAPE OF WASHINGTON LLC                 
      ##  39 MARINE LUMBER CO.                           
      ##  40 OREGON SANDBLASTING & COATING, INC          
      ##  41 SFW CONSTRUCTION LLC                        
      ##  42 SKY HEATING AND AIR CONDITIONING, INC.      
      ##  43 ARROW MECHANICAL CONTRACTORS, INC.          
      ##  44 CO OPERATIONS INC                           
      ##  45 SINGAPORE MATH INC                          
      ##  46 AKS ENGINEERING & FORESTRY LLC              
      ##  47 SPECIALIZED PAVEMENT MARKING, INC           
      ##  48 LIGHTSPEED TECHNOLOGIES INC.                
      ##  49 COLUMBIA CONSTRUCTION SERVICES, INC.        
      ##  50 CONTINENTAL MARKETING INC                   
      ##  51 DMT CLEAR GAS SOLUTIONS, LLC                
      ##  52 OSWEGO DRYWALL INSTALLERS INC               
      ##  53 PACIFIC INDUSTRIES, INC.                    
      ##  54 RINU  INCORPORATED                          
      ##  55 STAFFORD HILLS MANAGEMENT CO, LLC.          
      ##  56 THE SEABERG COMPANY, INC.                   
      ##  57 DAVID M KAO, M.D., P.C.                     
      ##  58 JUGS SPORTS INC.                            
      ##  59 MERIDIAN PARK RADIATION ONCOLOGY CENTER, IN…
      ##  60 OREGON AUTO SPRING SERVICE, INC.            
      ##  61 Q PACIFIC CORPORATION                       
      ##  62 PERLO CONSTRUCTION LLC                      
      ##  63 PERLO STRUCTURES LLC                        
      ##  64 SHIELDS MANUFACTURING INC.                  
      ##  65 LMC INC.                                    
      ##  66 AFP SYSTEMS INC                             
      ##  67 API INTERNATIONAL, INC.                     
      ##  68 BRIDGEWELL AGRIBUSINESS LLC                 
      ##  69 GRAMOR DEVELOPMENT, INC.                    
      ##  70 UNION WINE COMPANY                          
      ##  71 ABATEMENT SERVICES INCORPORATED             
      ##  72 ERIN ISLE CONSTRUCTION, INC.                
      ##  73 HELSER INDUSTRIES, INC.                     
      ##  74 LIGHTWORKS ELECTRIC COMPANY                 
      ##  75 PACIFIC SPORTS TURF INC                     
      ##  76 BREW ABYSS LLC                              
      ##  77 H.W. METAL PRODUCTS, INC.                   
      ##  78 TUALATIN COUNTRY CLUB                       
      ##  79 KREW KEGS, INC. DBA GOLPHER KEGS DBA G4 KEGS
      ##  80 SHAFFER & NELSON INC                        
      ##  81 AIREFCO, INC.                               
      ##  82 PRODAPT NORTH AMERICA, INC.                 
      ##  83 EROAD INC                                   
      ##  84 COMPUTER FORMS INC                          
      ##  85 HORIZON COMMUNITY CHURCH                    
      ##  86 LAKESIDE LUMBER, INC.                       
      ##  87 S&H LOGGING CO                              
      ##  88 SUBURBAN SUPPLY, INC.                       
      ##  89 TUALATIN AUTO BODY INC                      
      ##  90 AARON D GORIN  MD PC DBA GORIN PLASTIC SURG…
      ##  91 ADVANCED DERMATOLOGY OF OREGON, P.C.        
      ##  92 MODULAR MINI STORAGE INC                    
      ##  93 NOVA CASEWORK LLC                           
      ##  94 OREGON OCCUPATIONAL MEDICINE INC            
      ##  95 RUSSELL CONSTRUCTION INC                    
      ##  96 TIMOTHY P CONNALL MD PC                     
      ##  97 PNW FLATWORK, INC                           
      ##  98 WESTERN WOOD STRUCTURES, INC.               
      ##  99 ALBINA PIPE BENDING CO., INC.               
      ## 100 ALLIED TECHNOLOGY, INTERNATIONAL, LLC       
      ## 101 CASCADE COIL DRAPERY INC                    
      ## 102 CASCADE FUNERAL DIRECTORS, INC.             
      ## 103 FIVESPICE LLC                               
      ## 104 HARTMANN AND FORBES, INC                    
      ## 105 ROGERS ADMINISTRATION INC                   
      ## 106 RUSSELL GROUP LLC                           
      ## 107 WELLSOURCE, INC.                            
      ## 108 ALL AMERICAN BUILDING SERVICES LLC          
      ## 109 ALPENGLOW LLC                               
      ## 110 BURKSON TECHNOLOGIES INC                    
      ## 111 INDUSTRY INC                                
      ## 112 KEY MANUFACTURING AND RENTAL INC.           
      ## 113 MERINA & COMPANY, L.L.P.                    
      ## 114 PACIFIC LOCK+LOAD, INC.                     
      ## 115 KAI USA LTD.                                
      ## 116 CUI GLOBAL INC.                             
      ## 117 PACIFIC TRUCK COLORS INC.                   
      ## 118 CEDAR LANDSCAPE MAINTENANCE, LLC            
      ## 119 MECTA CORPORATION                           
      ## 120 UNIVERSAL RAZOR INDUSTRIES, LLC             
      ## 121 ALL STAR LABOR & STAFFING, LLC              
      ## 122 INTERACTIVE NORTHWEST, INC.                 
      ## 123 NORTHWEST DIE CASTING LLC                   
      ## 124 ROBERTS COFFEE LLC                          
      ## 125 THE NATT MCDOUGALL COMPANY                  
      ## 126 COURIER DIRECT, INC                         
      ## 127 CROWLEYS GRANITE CONCEPTS INC.              
      ## 128 D & D ACQUISITIONS, INC.                    
      ## 129 ELEMAR OREGON LLC                           
      ## 130 GCB SOLAR INC                               
      ## 131 MEXICALI EXPRESS, INC                       
      ## 132 PRO-GRO MIXES, INC.                         
      ## 133 SUBURBAN GRINDING, INC                      
      ## 134 WARRIOR INC                                 
      ## 135 EXPRESSO BUILDING SERVICES, LLC DBA EXPR    
      ## 136 OLSON BARKER CABINETS INC                   
      ## 137 AURORA LANDSCAPE LLC                        
      ## 138 ICON MEDICAL NETWORK LLC                    
      ## 139 PIZZA PRIDE, INC.                           
      ## 140 TRANSPAK INC                                
      ## 141 PARKER ENTERPRISES                          
      ## 142 XENIUM RESOURCES INC                        
      ## 143 BALZER PAINTING INC                         
      ## 144 BEVERAGE MANAGEMENT SYSTEMS, INC            
      ## 145 EASYPOWER LLC                               
      ## 146 RAYBORN'S PLUMBING INC                      
      ## 147 ROLLING HILLS COMMUNITY CHURCH              
      ## 148 TAURUS CONTROLS INC                         
      ## 149 THE STROLLER GROUP INC                      
      ## 150 3CM STONE INC                               
      ## 151 BACKGROUND INVESTIGATIONS INC.              
      ## 152 DELTA DRYWALL INC                           
      ## 153 DUNIGANS ELECTRIC, LLC                      
      ## 154 EASTERN STEEL ERECTORS, INC.                
      ## 155 ELLEN NONA HOYVEN PT PC                     
      ## 156 ROBERT TATSUMI MD LLC                       
      ## 157 SPACESAVER SPECIALISTS INC                  
      ## 158 XIOLOGIX, LLC                               
      ## 159 MITCH CHARTER SCHOOL                        
      ## 160 SELECT SALES INCORPORATED                   
      ## 161 NORTHWEST TRANSFER SERVICE INC              
      ## 162 ASSOCIATED WELDING AND MACHINE WORKS, IN    
      ## 163 MEGA FLUID SYSTEMS, INC                     
      ## 164 WYTEK CONTROLS INC                          
      ## 165 NORTHWEST DOOR & SUPPLY, INC.               
      ## 166 VERSALOGIC CORP.                            
      ## 167 HAYDEN'S GRILL, LLC                         
      ## 168 MEGA FLUID SYSTEMS, INC                     
      ## 169 ZINCFIVE INC                                
      ## 170 TROMLEY INDUSTRIAL HOLDINGS INC             
      ## 171 R. A. GRAY CONSTRUCTION, LLC
knitr::knit_exit()
```
