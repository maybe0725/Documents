Java XML XPath Sample
======================

### 1. Source
``````````````````````````````````````````````````````````````````````````````````````````````````````````````
import java.io.StringReader;

import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.xpath.XPath;
import javax.xml.xpath.XPathConstants;
import javax.xml.xpath.XPathFactory;
 
import org.w3c.dom.Document;
import org.w3c.dom.Node;
import org.w3c.dom.NodeList;
import org.xml.sax.InputSource;

public class XPathTest{
    
    public static void main(String[] args) throws Exception {
        
        StringBuffer xmlSB = new StringBuffer();
        
        xmlSB.append("  <root>                                      ");
        xmlSB.append("      <row>                                   ");
        xmlSB.append("          <col1 id='c1'>값1</col1>            ");
        xmlSB.append("          <col2 id='c2' val='val2'>값2</col2> ");
        xmlSB.append("      </row>                                  ");
        xmlSB.append("      <row>                                   ");
        xmlSB.append("          <col1 id='c3'>값3</col1>            ");
        xmlSB.append("          <col2 id='c4'>값4</col2>            ");
        xmlSB.append("      </row>                                  ");
        xmlSB.append("  </root>                                     ");
        
        // XML Document 객체 생성
        InputSource is = new InputSource(new StringReader(xmlSB.toString()));
        Document document = DocumentBuilderFactory.newInstance().newDocumentBuilder().parse(is);
 
        // 인터넷 상의 XML 문서는 요렇게 생성하면 편리함.
        /*
        Document document = DocumentBuilderFactory
                            .newInstance()
                            .newDocumentBuilder().parse("http://www.example.com/test.xml");
        */
        
        // xpath 생성
        XPath xpath = XPathFactory.newInstance().newXPath();
         
        System.out.println("==================================================");
        System.out.println("NodeList 가져오기 : row 아래에 있는 모든 col1 을 선택");
        System.out.println("기대값 : 값1   값3");
        System.out.println("--------------------------------------------------");
        NodeList cols = (NodeList)xpath.evaluate("//row/col1", document, XPathConstants.NODESET);
        for( int idx=0; idx<cols.getLength(); idx++ ){
            System.out.println(cols.item(idx).getTextContent());
        }
        System.out.println("==================================================\n\n");
         
        System.out.println("==================================================");
        System.out.println("id 가 c2 인 Node의 val attribute 값 가져오기");
        System.out.println("기대값 : val2");
        System.out.println("--------------------------------------------------");
        Node col2 = (Node)xpath.evaluate("//*[@id='c2']", document, XPathConstants.NODE);
        System.out.println(col2.getAttributes().getNamedItem("val").getTextContent());
        System.out.println("==================================================\n\n");
         
        System.out.println("==================================================");
        System.out.println("id 가 c3 인 Node 의 value 값 가져오기");
        System.out.println("기대값 : 값3");
        System.out.println("--------------------------------------------------");
        System.out.println(xpath.evaluate("//*[@id='c3']", document, XPathConstants.STRING));
        System.out.println("==================================================\n\n");
    }
}

``````````````````````````````````````````````````````````````````````````````````````````````````````````````

### 2. Output
``````````````````````````````````````````````````````````````````````````````````````````````````````````````
==================================================
NodeList 가져오기 : row 아래에 있는 모든 col1 을 선택
기대값 : 값1   값3
--------------------------------------------------
값1
값3
==================================================


==================================================
id 가 c2 인 Node의 val attribute 값 가져오기
기대값 : val2
--------------------------------------------------
val2
==================================================


==================================================
id 가 c3 인 Node 의 value 값 가져오기
기대값 : 값3
--------------------------------------------------
값3
==================================================
``````````````````````````````````````````````````````````````````````````````````````````````````````````````
