POI를 이용한 Excel (*.xls, *.xlsx) 읽기
======================================

### 프로젝트 파일 읽기.
`````````````````````````````````````````````````````````````````
String filePath = Test.class.getResource("").getPath();
XSSFWorkbook workbook = new XSSFWorkbook(new FileInputStream(new File(filePath + "template.xlsx")));
`````````````````````````````````````````````````````````````````

### *.xls 의 경우 excel 파일 읽어오는 방법
`````````````````````````````````````````````````````````````````
POIFSFileSystem fileSystem = new POIFSFileSystem(new FileInputStream(new File(excelFile)));
SSFWorkbook work = new HSSFWorkbook(fileSystem);

int sheetNum = work.getNumberOfSheets();

log.error("\n# sheet num : " + sheetNum); 

for( int loop = 0; loop < sheetNum; loop++){

    HSSFSheet sheet = work.getSheetAt(loop);
    int rows = sheet.getPhysicalNumberOfRows();

    log.error("\n# sheet rows num : " + rows);

    for( int rownum = 0; rownum < rows; rownum++){

        HSSFRow row = sheet.getRow(rownum);

        if(row != null){

            int cells = row.getPhysicalNumberOfCells();

            log.error("\n# row = " + row.getRowNum() + " / cells = " + cells);

            for(int cellnum =0; cellnum < cells; cellnum++){

                HSSFCell cell = row.getCell(cellnum);

                if(cell != null){

                    switch (cell.getCellType()) {

                        case HSSFCell.CELL_TYPE_FORMULA:
                            params.put("CELL_TYPE_FORMULA"+cellnum, cell.getNumericCellValue());
                            break;
                        case HSSFCell.CELL_TYPE_STRING:
                            params.put("CELL_TYPE_STRING"+cellnum, cell.getStringCellValue());
                            break;
                        case HSSFCell.CELL_TYPE_BLANK:
                            params.put("CELL_TYPE_BLANK"+cellnum, cell.getBooleanCellValue());
                            break;
                        case HSSFCell.CELL_TYPE_ERROR :
                            params.put("CELL_TYPE_ERROR"+cellnum, cell.getErrorCellValue());
                            break;
                            
                        ......
                        
                        default:
                            break;
                    }
                }

                log.error("\n CELL __ [params ] => " + params.toString());
            }
        }
    }
}
`````````````````````````````````````````````````````````````````

### *.xlsx 의 경우 excel 파일 읽어오는 방법
`````````````````````````````````````````````````````````````````
XSSFWorkbook work = new XSSFWorkbook(new FileInputStream(new File(excelFile)));

int sheetNum = work.getNumberOfSheets();

log.error("\n# sheet num : " + sheetNum);

for( int loop = 0; loop < sheetNum; loop++){

    XSSFSheet sheet = work.getSheetAt(loop);

    int rows = sheet.getPhysicalNumberOfRows();

    log.error("\n# sheet rows num : " + rows);

    for( int rownum = 0; rownum < rows; rownum++){

        XSSFRow row = sheet.getRow(rownum);

        if(row != null){

            int cells = row.getPhysicalNumberOfCells();

            log.error("\n# row = " + row.getRowNum() + " / cells = " + cells);

            for(int cellnum =0; cellnum < cells; cellnum++){

                XSSFCell cell = row.getCell(cellnum);

                if(cell != null){

                    switch (cell.getCellType()) {

                        case XSSFCell.CELL_TYPE_FORMULA:
                            params.put("CELL_TYPE_FORMULA"+cellnum, cell.getNumericCellValue());
                            break;
                        case XSSFCell.CELL_TYPE_STRING:
                            params.put("CELL_TYPE_STRING"+cellnum, cell.getStringCellValue());
                            break;
                        case HSSFCell.CELL_TYPE_BLANK:
                            params.put("CELL_TYPE_BLANK"+cellnum, cell.getBooleanCellValue());
                            break;
                        case XSSFCell.CELL_TYPE_ERROR :
                            params.put("CELL_TYPE_ERROR"+cellnum, cell.getErrorCellValue());
                            break;

                        ...... 

                        default:
                            break;
                    }
                }

                log.error("\n CELL __ [params ] => " + params.toString());

            }
        }
    }
}
`````````````````````````````````````````````````````````````````

### 에러사항
`````````````````````````````````````````````````````````````````
Invalid header signature; read 0x0010000000060409, expected 0xE11AB1A1E011CFD0

> DB에서 export 한 excel 의 경우는 header type 이 *csv 타입이라서 .. 에러가 발생

Exception occurred! : org.apache.poi.poifs.filesystem.OfficeXmlFileException: 
The supplied data appears to be in the Office 2007+ XML. 
You are calling the part of POI that deals with OLE2 Office Documents. 
You need to call a different part of POI to process this data (eg XSSF instead of HSSF)

> HSSFWorkbook 방식으로 *.xlsx 파일 읽기를 시도한 경우 
`````````````````````````````````````````````````````````````````
