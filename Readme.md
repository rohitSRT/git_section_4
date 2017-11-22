package com.aetna.enterprise.mmminquiry.service;

import java.io.BufferedReader;
import java.io.ByteArrayInputStream;
import java.io.DataOutputStream;
import java.io.File;
import java.io.FileInputStream;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStream;
import java.io.StringWriter;
import java.net.HttpURLConnection;
import java.net.URL;
import java.security.KeyStore;
import java.security.NoSuchAlgorithmException;
import java.security.SecureRandom;
import java.security.Security;
import java.util.ArrayList;
import java.util.List;

import javax.net.ssl.HttpsURLConnection;
import javax.net.ssl.KeyManager;
import javax.net.ssl.KeyManagerFactory;
import javax.net.ssl.SSLContext;
import javax.net.ssl.SSLSocketFactory;
import javax.net.ssl.TrustManager;
import javax.net.ssl.TrustManagerFactory;
import javax.net.ssl.X509TrustManager;
import javax.xml.bind.JAXBContext;
import javax.xml.bind.Marshaller;
import javax.xml.bind.Unmarshaller;

import org.apache.commons.httpclient.HttpClient;

import com.aetna.medicalManagement.mMMInquiry.DateRange;
import com.aetna.medicalManagement.mMMInquiry.ProgramDetailsByCumbIdCriteria;
import com.aetna.schema.caremanagement.activehealthmembersprogramparticipation.v1.ActiveHealthMembersProgramParticipationRequestType;
import com.aetna.schema.caremanagement.activehealthmembersprogramparticipation.v1.ActiveHealthMembersProgramParticipationResponseType;
import com.aetna.schema.caremanagement.activehealthmembersprogramparticipation.v1.CreateActiveHealthMembersProgramParticipationRequestType;

public class AhmServiceImpl implements AhmService {
	
	//private ActiveHealthMembersProgramParticipationRequestType activeHealthMembersProgramParticipationRequestType;
	//private CreateActiveHealthMembersProgramParticipationRequestType createActiveHealthMembersProgramParticipationRequestType;
	private DateRange dateRange;
	@Override
	public ActiveHealthMembersProgramParticipationRequestType mapInput(
			ProgramDetailsByCumbIdCriteria programDetailsByCumbIdCriteria) {
		//List<Integer> spcPgmOccNum = new ArrayList<Integer>();
		//spcPgmOccNum.add(201);
		//createActiveHealthMembersProgramParticipationRequestType.setSpcPgmOccNum(spcPgmOccNum);
		ActiveHealthMembersProgramParticipationRequestType activeHealthMembersProgramParticipationRequestType = new ActiveHealthMembersProgramParticipationRequestType();
		CreateActiveHealthMembersProgramParticipationRequestType createActiveHealthMembersProgramParticipationRequestType = new CreateActiveHealthMembersProgramParticipationRequestType();
		createActiveHealthMembersProgramParticipationRequestType.setCumbid(programDetailsByCumbIdCriteria.getCumbId());
		createActiveHealthMembersProgramParticipationRequestType.setFrom(programDetailsByCumbIdCriteria.getDateRange().getFrom());
		createActiveHealthMembersProgramParticipationRequestType.setTo(programDetailsByCumbIdCriteria.getDateRange().getTo());
		createActiveHealthMembersProgramParticipationRequestType.setExternalVndPrgId(programDetailsByCumbIdCriteria.getExternalVndPrgId().getExternalVendProgId());
		//List<String> strList = new ArrayList<String>();
		//strList.addAll(programDetailsByCumbIdCriteria.getSpcPgmOccNum().getSpecProgOccNum());
				List<Integer> spcPgmOccNumInteger = new ArrayList<Integer>();
				for(String s : programDetailsByCumbIdCriteria.getSpcPgmOccNum().getSpecProgOccNum()) spcPgmOccNumInteger.add(Integer.valueOf(s));
				
		createActiveHealthMembersProgramParticipationRequestType.setSpcPgmOccNum(spcPgmOccNumInteger);
		
		activeHealthMembersProgramParticipationRequestType.setCreateActiveHealthMembersProgramParticipationRequest(createActiveHealthMembersProgramParticipationRequestType);
//		createActiveHealthMembersProgramParticipationRequestType.setFrom(programDetailsByCumbIdCriteria.getDateRange().getFrom());
//		createActiveHealthMembersProgramParticipationRequestType.setTo(programDetailsByCumbIdCriteria.getDateRange().getTo());
		
		//************************************Need to refactor fromm here ****************************************//*
		try{
			
            JAXBContext   jaxbContext = JAXBContext.newInstance("com.aetna.schema.caremanagement.activehealthmembersprogramparticipation.v1");
            Marshaller marshaller=jaxbContext.createMarshaller();
            StringWriter strXmlWriter = new StringWriter();
            marshaller.marshal(activeHealthMembersProgramParticipationRequestType, strXmlWriter);
            String strRequestXML = strXmlWriter.toString();

			JAXBContext   jaxbContext2 = JAXBContext.newInstance("com.aetna.schema.caremanagement.activehealthmembersprogramparticipation.v1");
			Unmarshaller unMarshaller= jaxbContext2.createUnmarshaller();
			
			
		  URL url = new URL("https://corep2gnrl2.devc.aetna.com/edb/ActiveHealth/v1");
				 HttpsURLConnection cons = (HttpsURLConnection) url.openConnection();
				 
				 ActiveHealthMembersProgramParticipationResponseType activeHealthMembersProgramParticipationResponseType = new ActiveHealthMembersProgramParticipationResponseType();
	       cons.setDoOutput(true);
	        cons.setRequestMethod("POST");
	        cons.setRequestProperty("Accept", "application/xml");
	        //cons.setRequestProperty("SSLKeystore", "DPTester.p12");
	      /*  String xmlContent = "<tns:ActiveHealthMembersProgramParticipationRequest"+
                    "xsi:schemaLocation="+"http://www.schema.aetna.com/caremanagement/activehealthmembersprogramparticipation/v1 ActiveHealthMembersProgramParticipation_v1.xsd "
                    +"<tns:eieHeader>"+
"<p:transactionID>AHP123</p:transactionID>"+ "</tns:eieHeader>"+
                     " <tns:createActiveHealthMembersProgramParticipationRequest>"+ 
"<tns:cumbid>00368277</tns:cumbid>"+
         "<tns:from>2017-10-04</tns:from>"+
        "<tns:to>2018-01-05</tns:to>"+
         "<tns:spcPgmOccNum>201</tns:spcPgmOccNum>"+
        " <tns:spcPgmOccNum>202</tns:spcPgmOccNum>"+
        " <tns:spcPgmOccNum>203</tns:spcPgmOccNum>"+
        " <tns:externalVndPrgId>1</tns:externalVndPrgId>"+
"<tns:externalVndPrgId>2</tns:externalVndPrgId>"+
"<tns:externalVndPrgId>3</tns:externalVndPrgId>"+
" </tns:createActiveHealthMembersProgramParticipationRequest>"+
" </tns:ActiveHealthMembersProgramParticipationRequest>" ;*/
	        
            OutputStream os=cons.getOutputStream();
            os.write((strRequestXML).getBytes("UTF-8"));
            
            if (cons.getResponseCode() != 200) {
                      throw new RuntimeException("Failed : HTTP error code : "
                          + cons.getResponseCode());
                      }
                  System.out.println(cons.getResponseCode());

                  
                  BufferedReader in = new BufferedReader(
                              new InputStreamReader(cons.getInputStream()));
                      String inputLine;
                      StringBuffer response1 = new StringBuffer();

                      while ((inputLine = in.readLine()) != null) {
                          response1.append(inputLine);
                      }

                  System.out.println(response1.toString());
                  
                  InputStream is = new ByteArrayInputStream(response1.toString().getBytes("UTF-8")); 
                  activeHealthMembersProgramParticipationResponseType = (ActiveHealthMembersProgramParticipationResponseType) unMarshaller.unmarshal(is);


                  	System.out.println(cons.getResponseCode());
        
	        
		} catch (Exception e)    {
			e.printStackTrace();
		}
	        
	   
				return activeHealthMembersProgramParticipationRequestType;
                          
}



	
	

}
