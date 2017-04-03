--- 
 
# required metadata 
title: " Set info such as a description " 
description: " Set .xdf file or data frame information, such as a description " 
keywords: "RevoScaleR, rxSetInfo, rxSetInfoXdf,  attribute " 
author: "richcalaway" 
manager: "jhubbard" 
ms.date: "03/23/2017" 
ms.topic: "reference" 
ms.prod: "microsoft-r" 
ms.service: "" 
ms.assetid: "" 
 
# optional metadata 
ROBOTS: "" 
audience: "" 
ms.devlang: "" 
ms.reviewer: "" 
ms.suite: "" 
ms.tgt_pltfrm: "" 
ms.technology: "r-server" 
ms.custom: "" 
 
--- 
 
 
 
 #`rxSetInfo`:  Set info such as a description 

 Applies to version {PRODUCT_VERSION} of package RevoScaleR.
 
 ##Description
 
Set .xdf file or data frame information, such as a description
 
 
 ##Usage

```   
  rxSetInfo(data, description = "")
  rxSetInfoXdf(file, description = "")
 
```
 
 
 ##Arguments

   
    
 ### `data`
  An .xdf file name, an [RxXdfData](RxXdfData.md) object, or a data frame.  
  
    
 ### `file`
  An .xdf file name or an [RxXdfData](RxXdfData.md) object  
  
  
  
 ### `description`
  character string containing the file or data frame description  
  
 
 
 ##Value
 
An [RxXdfData](RxXdfData.md) object representing the .xdf file, or a new
data frame with the `.rxDescription` attribute set to the
specified `description`.
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[rxGetInfo](rxGetInfoXdf.md),
[rxGetVarInfo](rxGetVarInfoXdf.md).
   
 
 ##Examples

 ```
   
   
  # Create a sample data frame and .xdf file
  outFile <- tempfile(pattern= ".rxTempFile", fileext = ".xdf")
  if (file.exists(outFile)) file.remove(outFile)
        
  origData <- data.frame(x=1:10)
  rxDataStep(inData = origData, outFile = outFile, rowsPerRead = 5)
      
  # Set a description in the data file
  myDescription <- "Hello Data File!"
  rxSetInfo( data = outFile, description = myDescription)
  rxGetInfo(outFile)
      
  # Read a data frame out of the output file    
  myData <- rxDataStep(outFile )
  # Look at its attribute
  attr(myData, ".rxDescription")    
        
  file.remove(outFile)
      
  myNewDescription = "Good-by data."
  myData <- rxSetInfo(data=myData, description = myNewDescription)
  rxGetInfo(myData)
  
 
```
 
 
 
 