# OD_Kynetics_Survival_percentage
## Scripts de SEC para calcular el porcentaje de células vivas en un experimento de CLS usando el TECAN M1000.
### Sigue el método de Murakami 2008
  
  Para usarlo primero se definen los parámetros
```  
pls= 1:7; %number of plates
od = .3; % od at interpolation ORIGINAL .28
odTh = -0.3; %ORIGINAL -0.3 cuánto tiene que bajar la OD para que se considere un día nuevo
n0 = .15; % minimum OD for GR ORIGINAL .18
nt = .45; % Max OD for GR ORIGINAL .48
```

Quitamos el ruido de fondo contenido en la variable "value"
```
 bgdataClean = bkgsubstractionOD(BgDataAll,value); %quita la OD de fondo
```

Calcular la tasa de crecimiento para cada día de outgrowth
```
[timeOD] = getimeAA(bgdataClean,pls,od,odTh); %Gets matrix with time at od stated above for each well and plate
[gwrate] = getgr(bgdataClean,pls,n0,nt,odTh); %Gets growth rate matrix
```

Calcular survival que contiene .t (con los tiempos en días) y .s (porcentajes de supervivencia)
```
[survival] = getsurv(timeOD,gwrate,pls) %Gets survival percentage matrix
survival = getxvec(bgdataClean,survival,pls,od,odTh) %Gets matrix into survival structure for time at which interpolation was made in days and plots percentage vs time
```

Calcular tasa de muerte exponencial
```
[decayRate] = getExponentialRate(survival);```
```
