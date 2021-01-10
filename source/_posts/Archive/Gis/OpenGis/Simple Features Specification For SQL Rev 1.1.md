## 目录

### 0 PREFACE                                             0-1 

0.1 SUBMITTING COMPANIES                         0-1 
0.2 SUBMISSION CONTACT POINTS              0-1 
0.3 DOCUMENT CONVENTIONS                     0-2 
0.4 REVISION HISTORY                                     0-2 
0.5 EDITORIAL NOTES                                       0-3 

### 1 OVERVIEW                                          

 1-1 1.1 APPROACH                                          1-1 

### 2 ARCHITECTURE 建筑                                      2-1 

2.1 GEOMETRY OBJECT MODEL 几何对象模型                                 . 2-1 
2.1.1 Geometry 几何                                       2-2 
2.1.2 Geometry Collection  几何集合                                . 2-4 
2.1.3 Point 点                                           2-4 
2.1.4 MultiPoint 复数点                            2-4 
2.1.5 Curve 网格                                         2-5 
2.1.6 LineString, Line, LinearRing ？？？，线，线性环                              . 2-5 
2.1.7 MultiCurve  复数网格  2-6 
2.1.8 MultiLineString 复数线  2-7 
2.1.9 Surface 表面  2-7 
2.1.10 Polygon  几何  2-8 
2.1.11 MultiSurface   2-10 
2.1.12 MultiPolygon   2-10 
2.1.13 Relational Operators 相关运算符                                  2-12 
2.2 ARCHITECTURE—SQL92 IMPLEMENTATION OF FEATURE TABLES 体系结构—要素表的SQL92实现  2-20  
2.2.1 Feature Table Metadata Views  功能表元数据视图                           . 2-21 
2.2.2 Geometry Columns Metadata Views 几何列元数据视图                          2-21 
2.2.3 Spatial Reference System Information Views 空间参考系统信息视图     2-21 
2.2.4 Feature Tables and Views  功能表和视图                              2-22 
2.2.5 Geometry and Geometric Element Views  几何和几何元素视图   2-22 
2.2.6 Notes on SQL92 data types 有关SQL92数据类型的注释  2-23 
2.2.7 Notes on ODBC Access to Geometry Values stored in Binary form 关于ODBC访问以二进制形式存储的几何值的说明 2-24 
2.3 ARCHITECTURE—SQL92 WITH GEOMETRY TYPES IMPLEMENTATION OF FEATURE TABLES  体系结构—具有几何类型的功能表的SQL92实现 . 2-24 
2.3.1 Feature Table Metadata Views 要素表元数据视图 . 2-24 
2.3.2 Geometry Columns Metadata Views 几何列元数据视图 2-24   
2.3.3 Spatial Reference System Information Views 空间参考系统信息视图 2-24  
2.3.4 Feature Tables and Views 功能表和视图 2-25 
Page ii 2.3.5 Background Information on SQL Abstract Data Types 有关SQL抽象数据类型的背景信息 2-25 
2.3.6 Scope of this OpenGIS Geometry Types specification 本OpenGIS Geometry Types规范的范围 2-25 
2.3.7 SQL Geometry Type Hierarchy  SQL几何类型层次结构 2-26 
2.3.8 Geometry Values and Spatial Reference Systems  几何值和空间参考系统 2-27 
2.3.9 ODBC Access to Geometry Values in the SQL with Geometry Types case 带有几何类型的SQL中对几何值的ODBC访问  2-28 

### 3 COMPONENT SPECIFICATIONS                

 . 3-1 3.1 COMPONENTS—SQL92 IMPLEMENTATION OF FEATURE TABLES               . 3-1 
3.1.1 Spatial Reference System Information                         
.. 3-1 
3.1.2 Geometry Columns Metadata View                            
3-2 
3.1.3 Feature Tables and Views                               
.. 3-4 
3.1.4 Geometry Tables or Views                                
. 3-4 
3.1.5 Operators                                       
.. 3-7 3.2 COMPONENTS—SQL92 WITH GEOMETRY TYPES IMPLEMENTATION OF FEATURE TABLES    
3-7 3.2.1 Spatial Reference System Information View                        
3-7 3.2.2 Geometry Columns Metadata View                            
3-8 3.2.3 SQL Geometry Types                                   
3-8 3.2.4 Feature Tables and Views                                
3-10 3.2.5 SQL Textual Representation of Geometry                       
. 3-11 3.2.6 SQL Functions for Constructing a Geometry Value given its Well-known Text Representation                                         
.. 3-12 3.2.7 SQL Functions for Constructing a Geometry Value given its Well-known Binary Representation                                         
.. 3-14 3.2.8 SQL functions for obtaining the Well-known Text Representation of a Geometry    
.. 3-15 3.2.9 SQL functions for obtaining the Well-known Binary Representation of a Geometry  
.. 3-15 3.2.10 SQL Functions on Type Geometry                            
3-16 3.2.11 SQL Functions on Type Point                               
3-17 3.2.12 SQL Functions on Type Curve                             
.. 3-17 3.2.13 SQL Functions on Type LineString                           
. 3-18 3.2.14 SQL Functions on Type Surface                             
3-18 3.2.15 SQL Functions on Type Polygon                           
.. 3-19 3.2.16 SQL Functions on Type GeomCollection                       
.. 3-19 3.2.17 SQL Functions on Type MultiCurve                            
3-19 3.2.18 SQL Functions on Type MultiSurface                         
. 3-20 3.2.19 SQL functions that test Spatial Relationships                      
. 3-20 3.2.20 SQL Functions for Distance Relationships                       
.. 3-22 3.2.21 SQL Functions that implement Spatial Operators                   
. 3-23 3.2.22 SQL Function usage and References to Geometry                     
3-24 3.3 THE WELL-KNOWN BINARY REPRESENTATION FOR GEOMETRY (WKBGEOMETRY)     . 3-24 3.3.1 Component Overview                                 
. 3-24 3.3.2 Component Description  组件说明                               
3-24 3.4 WELL-KNOWN TEXT REPRESENTATION OF SPATIAL REFERENCE SYSTEMS  空间参考系统的知名文本表示         
3-28 3.4.1 Component Overview                                 
. 3-28 3.4.2 Component Description                                 
3-28 

### 4 SUPPORTED SPATIAL REFERENCE DATA                         

. 4-1 4.1 SUPPORTED LINEAR UNITS                                   
4-1 4.2 SUPPORTED ANGULAR UNITS                                
.. 4-1 4.3 SUPPORTED SPHEROIDS                                   
.. 4-1 4.4 SUPPORTED GEODETIC DATUMS                              
 . 4-2 4.5 SUPPORTED PRIME MERIDIANS                                 
4-3 4.6 SUPPORTED MAP PROJECTIONS                                
.. 4-3 4.7 MAP PROJECTION PARAMETERS                                
. 4-3 

### 5 REFERENCES                                          

. 5-1