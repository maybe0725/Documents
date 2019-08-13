POI QUICK GUIDE
===============

### 새로운 통합문서생성
```````````````````````````````````````````````````````````````````````
    HSSFWorkbook wb = new HSSFWorkbook();
    FileOutputStream fileOut = new FileOutputStream("workbook.xls");
    wb.write(fileOut);
    fileOut.close();
```````````````````````````````````````````````````````````````````````

### 새로운 스프레드시트 생성
```````````````````````````````````````````````````````````````````````
    HSSFWorkbook wb = new HSSFWorkbook();
    HSSFSheet sheet1 = wb.createSheet("new sheet");
    HSSFSheet sheet2 = wb.createSheet("second sheet");
    FileOutputStream fileOut = new FileOutputStream("workbook.xls");
    wb.write(fileOut);
    fileOut.close();
```````````````````````````````````````````````````````````````````````

### 셀의 생성
```````````````````````````````````````````````````````````````````````
    HSSFWorkbook wb = new HSSFWorkbook();
    HSSFSheet sheet = wb.createSheet("new sheet");

    // Create a row and put some cells in it. Rows are 0 based.
    HSSFRow row = sheet.createRow((short)0);
    // Create a cell and put a value in it.
    HSSFCell cell = row.createCell((short)0);
    cell.setCellValue(1);

    // Or do it on one line.
    row.createCell((short)1).setCellValue(1.2);
    row.createCell((short)2).setCellValue("This is a string");
    row.createCell((short)3).setCellValue(true);

    // Write the output to a file
    FileOutputStream fileOut = new FileOutputStream("workbook.xls");
    wb.write(fileOut);
    fileOut.close();
```````````````````````````````````````````````````````````````````````

### 날짜타입의 셀의 생성
```````````````````````````````````````````````````````````````````````
    HSSFWorkbook wb = new HSSFWorkbook();
    HSSFSheet sheet = wb.createSheet("new sheet");

    // Create a row and put some cells in it. Rows are 0 based.
    HSSFRow row = sheet.createRow((short)0);

    // Create a cell and put a date value in it.  The first cell is not styled
    // as a date.
    HSSFCell cell = row.createCell((short)0);
    cell.setCellValue(new Date());

    // we style the second cell as a date (and time).  It is important to
    // create a new cell style from the workbook otherwise you can end up
    // modifying the built in style and effecting not only this cell but other cells.
    HSSFCellStyle cellStyle = wb.createCellStyle();
    cellStyle.setDataFormat(HSSFDataFormat.getFormat("m/d/yy h:mm"));
    cell = row.createCell((short)1);
    cell.setCellValue(new Date());
    cell.setCellStyle(cellStyle);

    // Write the output to a file
    FileOutputStream fileOut = new FileOutputStream("workbook.xls");
    wb.write(fileOut);
    fileOut.close();
```````````````````````````````````````````````````````````````````````

### 타입이 다른 셀을 함께 작업하기
```````````````````````````````````````````````````````````````````````
    HSSFWorkbook wb = new HSSFWorkbook();
    HSSFSheet sheet = wb.createSheet("new sheet");
    HSSFRow row = sheet.createRow((short)2);
    row.createCell((short) 0).setCellValue(1.1);
    row.createCell((short) 1).setCellValue(new Date());
    row.createCell((short) 2).setCellValue("a string");
    row.createCell((short) 3).setCellValue(true);
    row.createCell((short) 4).setCellType(HSSFCell.CELL_TYPE_ERROR);

    // Write the output to a file
    FileOutputStream fileOut = new FileOutputStream("workbook.xls");
    wb.write(fileOut);
    fileOut.close();
```````````````````````````````````````````````````````````````````````

### 다양한 정렬옵션 데모
```````````````````````````````````````````````````````````````````````
    public static void main(String[] args)
            throws IOException
    {
        HSSFWorkbook wb = new HSSFWorkbook();
        HSSFSheet sheet = wb.createSheet("new sheet");
        HSSFRow row = sheet.createRow((short) 2);
        createCell(wb, row, (short) 0, HSSFCellStyle.ALIGN_CENTER);
        createCell(wb, row, (short) 1, HSSFCellStyle.ALIGN_CENTER_SELECTION);
        createCell(wb, row, (short) 2, HSSFCellStyle.ALIGN_FILL);
        createCell(wb, row, (short) 3, HSSFCellStyle.ALIGN_GENERAL);
        createCell(wb, row, (short) 4, HSSFCellStyle.ALIGN_JUSTIFY);
        createCell(wb, row, (short) 5, HSSFCellStyle.ALIGN_LEFT);
        createCell(wb, row, (short) 6, HSSFCellStyle.ALIGN_RIGHT);

        // Write the output to a file
        FileOutputStream fileOut = new FileOutputStream("workbook.xls");
        wb.write(fileOut);
        fileOut.close();

    }

    /**
     * Creates a cell and aligns it a certain way.
     *
     * @param wb        the workbook
     * @param row       the row to create the cell in
     * @param column    the column number to create the cell in
     * @param align     the alignment for the cell.
     */
    private static void createCell(HSSFWorkbook wb, HSSFRow row, short column, short align)
    {
        HSSFCell cell = row.createCell(column);
        cell.setCellValue("Align It");
        HSSFCellStyle cellStyle = wb.createCellStyle();
        cellStyle.setAlignment(align);
        cell.setCellStyle(cellStyle);
    }
```````````````````````````````````````````````````````````````````````

### 테두리 다루기
```````````````````````````````````````````````````````````````````````
    HSSFWorkbook wb = new HSSFWorkbook();
    HSSFSheet sheet = wb.createSheet("new sheet");

    // Create a row and put some cells in it. Rows are 0 based.
    HSSFRow row = sheet.createRow((short) 1);

    // Create a cell and put a value in it.
    HSSFCell cell = row.createCell((short) 1);
    cell.setCellValue(4);

    // Style the cell with borders all around.
    HSSFCellStyle style = wb.createCellStyle();
    style.setBorderBottom(HSSFCellStyle.BORDER_THIN);
    style.setBottomBorderColor(HSSFColor.BLACK.index);
    style.setBorderLeft(HSSFCellStyle.BORDER_THIN);
    style.setLeftBorderColor(HSSFColor.GREEN.index);
    style.setBorderRight(HSSFCellStyle.BORDER_THIN);
    style.setRightBorderColor(HSSFColor.BLUE.index);
    style.setBorderTop(HSSFCellStyle.BORDER_MEDIUM_DASHED);
    style.setTopBorderColor(HSSFColor.BLACK.index);
    cell.setCellStyle(style);

    // Write the output to a file
    FileOutputStream fileOut = new FileOutputStream("workbook.xls");
    wb.write(fileOut);
    fileOut.close();
```````````````````````````````````````````````````````````````````````

### 색채우기와 글꼴색
```````````````````````````````````````````````````````````````````````
    HSSFWorkbook wb = new HSSFWorkbook();
    HSSFSheet sheet = wb.createSheet("new sheet");

    // Create a row and put some cells in it. Rows are 0 based.
    HSSFRow row = sheet.createRow((short) 1);

    // Aqua background
    HSSFCellStyle style = wb.createCellStyle();
    style.setFillBackgroundColor(HSSFColor.AQUA.index);
    style.setFillPattern(HSSFCellStyle.BIG_SPOTS);
    HSSFCell cell = row.createCell((short) 1);
    cell.setCellValue("X");
    cell.setCellStyle(style);

    // Orange "foreground", foreground being the fill foreground not the font color.
    style = wb.createCellStyle();
    style.setFillForegroundColor(HSSFColor.ORANGE.index);
    style.setFillPattern(HSSFCellStyle.SOLID_FOREGROUND);
    cell = row.createCell((short) 2);
    cell.setCellValue("X");
    cell.setCellStyle(style);

    // Write the output to a file
    FileOutputStream fileOut = new FileOutputStream("workbook.xls");
    wb.write(fileOut);
    fileOut.close();
```````````````````````````````````````````````````````````````````````

### 셀 병합
```````````````````````````````````````````````````````````````````````
    HSSFWorkbook wb = new HSSFWorkbook();
    HSSFSheet sheet = wb.createSheet("new sheet");

    HSSFRow row = sheet.createRow((short) 1);
    HSSFCell cell = row.createCell((short) 1);
    cell.setCellValue("This is a test of merging");

    sheet.addMergedRegion(new Region(1,(short)1,1,(short)2));

    // Write the output to a file
    FileOutputStream fileOut = new FileOutputStream("workbook.xls");
    wb.write(fileOut);
    fileOut.close();
```````````````````````````````````````````````````````````````````````

### 폰트 다루기
```````````````````````````````````````````````````````````````````````
    HSSFWorkbook wb = new HSSFWorkbook();
    HSSFSheet sheet = wb.createSheet("new sheet");

    // Create a row and put some cells in it. Rows are 0 based.
    HSSFRow row = sheet.createRow((short) 1);

    // Create a new font and alter it.
    HSSFFont font = wb.createFont();
    font.setFontHeightInPoints((short)24);
    font.setFontName("Courier New");
    font.setItalic(true);
    font.setStrikeout(true);

    // Fonts are set into a style so create a new one to use.
    HSSFCellStyle style = wb.createCellStyle();
    style.setFont(font);

    // Create a cell and put a value in it.
    HSSFCell cell = row.createCell((short) 1);
    cell.setCellValue("This is a test of fonts");
    cell.setCellStyle(style);

    // Write the output to a file
    FileOutputStream fileOut = new FileOutputStream("workbook.xls");
    wb.write(fileOut);
    fileOut.close();
```````````````````````````````````````````````````````````````````````

### 통합문서 읽고 쓰기
```````````````````````````````````````````````````````````````````````
    POIFSFileSystem fs      =
            new POIFSFileSystem(new FileInputStream("workbook.xls"));
    HSSFWorkbook wb = new HSSFWorkbook(fs);
    HSSFSheet sheet = wb.getSheetAt(0);
    HSSFRow row = sheet.getRow(2);
    HSSFCell cell = row.getCell((short)3);
    if (cell == null)
        cell = row.createCell((short)3);
    cell.setCellType(HSSFCell.CELL_TYPE_STRING);
    cell.setCellValue("a test");

    // Write the output to a file
    FileOutputStream fileOut = new FileOutputStream("workbook.xls");
    wb.write(fileOut);
    fileOut.close();
```````````````````````````````````````````````````````````````````````

### 셀에서의 개행문자 사용하기
```````````````````````````````````````````````````````````````````````
    HSSFWorkbook wb = new HSSFWorkbook();
    HSSFSheet s = wb.createSheet();
    HSSFRow r = null;
    HSSFCell c = null;
    HSSFCellStyle cs = wb.createCellStyle();
    HSSFFont f = wb.createFont();
    HSSFFont f2 = wb.createFont();

    cs = wb.createCellStyle();

    cs.setFont( f2 );
    //Word Wrap MUST be turned on
    cs.setWrapText( true );

    r = s.createRow( (short) 2 );
    r.setHeight( (short) 0x349 );
    c = r.createCell( (short) 2 );
    c.setCellType( HSSFCell.CELL_TYPE_STRING );
    c.setCellValue( "Use \n with word wrap on to create a new line" );
    c.setCellStyle( cs );
    s.setColumnWidth( (short) 2, (short) ( ( 50 * 8 ) / ( (double) 1 / 20 ) ) );

    FileOutputStream fileOut = new FileOutputStream( "workbook.xls" );
    wb.write( fileOut );
    fileOut.close();
```````````````````````````````````````````````````````````````````````

### 데이터타입
```````````````````````````````````````````````````````````````````````
    HSSFWorkbook wb = new HSSFWorkbook();
    HSSFSheet sheet = wb.createSheet("format sheet");
    HSSFCellStyle style;
    HSSFDataFormat format = wb.createDataFormat();
    HSSFRow row;
    HSSFCell cell;
    short rowNum = 0;
    short colNum = 0;

    row = sheet.createRow(rowNum++);
    cell = row.createCell(colNum);
    cell.setCellValue(11111.25);
    style = wb.createCellStyle();
    style.setDataFormat(format.getFormat("0.0"));
    cell.setCellStyle(style);

    row = sheet.createRow(rowNum++);
    cell = row.createCell(colNum);
    cell.setCellValue(11111.25);
    style = wb.createCellStyle();
    style.setDataFormat(format.getFormat("#,##0.0000"));
    cell.setCellStyle(style);

    FileOutputStream fileOut = new FileOutputStream("workbook.xls");
    wb.write(fileOut);
    fileOut.close();
```````````````````````````````````````````````````````````````````````

### 스프레드시트내의 인쇄범위설정법
```````````````````````````````````````````````````````````````````````
    HSSFWorkbook wb = new HSSFWorkbook();
    HSSFSheet sheet = wb.createSheet("format sheet");
    HSSFPrintSetup ps = sheet.getPrintSetup()
    
    sheet.setAutobreaks(true)
    
    ps.setFitHeight((short)1);
    ps.setFitWidth((short)1);


    // Create various cells and rows for spreadsheet.

    FileOutputStream fileOut = new FileOutputStream("workbook.xls");
    wb.write(fileOut);
    fileOut.close();
```````````````````````````````````````````````````````````````````````

### 스프레드시트의 바닥글에 페이지번호 설정법
```````````````````````````````````````````````````````````````````````
    HSSFWorkbook wb = new HSSFWorkbook();
    HSSFSheet sheet = wb.createSheet("format sheet");
    HSSFFooter footer = sheet.getFooter()
    
    footer.setRight( "Page " + HSSFFooter.page() + " of " + HSSFFooter.numPages() );
    


    // Create various cells and rows for spreadsheet.

    FileOutputStream fileOut = new FileOutputStream("workbook.xls");
    wb.write(fileOut);
    fileOut.close();
```````````````````````````````````````````````````````````````````````
