Priy-R
======

Sample CodeBase


package com;
/**
 * 
 */

/**
 * @author Priy Ranjan
 * 
 * This class holds the company name, 
 * max share price, its year and month 
 * at any given time.
 */
public class CompanyDetails
{
	private String companyName;
	private int maxSharePrice;
	private int year;
	private String month;
	
	/**
	 * 
	 * @return company name
 	 */
	public String getCompanyName() 
	{
		return companyName;
	}
	
	/**
	 * sets the company name
	 * @param companyName
	 */
	public void setCompanyName(String companyName) 
	{
		this.companyName = companyName;
	}
	
	/**
	 * 
	 * @return the max share price
	 */
	public int getMaxSharePrice() 
	{
		return maxSharePrice;
	}
	
	/**
	 * sets the max share price
	 * @param maxSharePrice
	 */
	public void setMaxSharePrice(int maxSharePrice) 
	{
		this.maxSharePrice = maxSharePrice;
	}
	
	/**
	 * 
	 * @return the year
	 */
	public int getYear() 
	{
		return year;
	}
	
	/**
	 * sets the year
	 * @param year
	 */
	public void setYear(int year) 
	{
		this.year = year;
	}
	
	/**
	 * 
	 * @return the month
	 */
	public String getMonth() 
	{
		return month;
	}
	
	/**
	 * sets the month
	 * @param month
	 */
	public void setMonth(String month) 
	{
		this.month = month;
	}
}


package com;
/**
 * 
 */

/**
 * @author Priy Ranjan
 * 
 * This class has some helper methods.
 * It has the methods to parse the input line (one at a time)
 * and fill the appropriate object(s). (CSVLinePresentation & CompanyDetails)
 */
public class CSVLineParser
{
	
	/**
	 * Given an input line it fills a given CSVLinePresentation object. 
	 * 
	 * @param inputLine_in
	 * @param aLineInputobj_in
	 */
	public static void updateLinePresentation(String inputLine_in, CSVLinePresentation aLineInputobj_in)
	{
		if(inputLine_in == null || inputLine_in.trim().equals(""))
			return ;
		
		if(aLineInputobj_in == null)
			return;
		
		String[] values = inputLine_in.split(",");
		
		// year, month and at least one company must be present.
		if(values.length < 3)
		{
			System.out.println("\" "+ inputLine_in + "\" is not properly formatted");
			return;
		}
		
		try
		{
			aLineInputobj_in.setYear(Integer.parseInt(values[0].trim()));
		}
		catch(NumberFormatException e)
		{
			System.out.println("\" "+ inputLine_in + "\" is not properly formatted");
			return;
		}
		
		aLineInputobj_in.setMonth(values[1].trim());
		
		aLineInputobj_in.setSharePrices(new int[values.length - 2]);
		for(int i=2; i<values.length; i++)
		{
			int[] sharePrices = aLineInputobj_in.getSharePrices(); 
			try
			{
				sharePrices[i-2] = Integer.parseInt(values[i].trim());
			}
			catch(NumberFormatException e)
			{
				System.out.println("\" "+ inputLine_in + "\" is not properly formatted");
			}
		}
	}
	
	
	
	/**
	 * Fills the company names in Company Details object 
	 * from the header of CSV file.
	 * 
	 * @param inputLine_in
	 */
	public static CompanyDetails[] fillCompanyNames(String inputLine_in)
	{
		if(inputLine_in == null || inputLine_in.trim().equals(""))
			return null;
		
		String[] values = inputLine_in.split(",");
		
		// year, month and at least one company must be present.
		if(values.length < 3)
		{
			System.out.println("\" "+ inputLine_in + "\" is not properly formatted");
			return null;		
		}
		
		CompanyDetails[] compDetailsArr = new CompanyDetails[values.length-2]; 
		
		for(int i=2; i<values.length; i++)
		{
			CompanyDetails cd = new CompanyDetails();
			cd.setCompanyName(values[i].trim());
			compDetailsArr[i-2] = cd;
		}
		
		return compDetailsArr;
	}
	
	
	
	/**
	 * Updates the company detail object if given line object has higher
	 * share price. Then it updates the line object values to Company Details object.
	 * 
	 * @param aLineInputobj_in
	 * @param cd_in
	 */
	public static void updateCompanyDetails(CSVLinePresentation aLineInputobj_in, CompanyDetails[] cd_in)
	{
		int[] sharePrices = aLineInputobj_in.getSharePrices();
		for(int i=0; i<sharePrices.length; i++)
		{
			int maxSharePrice = cd_in[i].getMaxSharePrice();
			if(sharePrices[i] > maxSharePrice)
			{
				cd_in[i].setMaxSharePrice(sharePrices[i]);
				cd_in[i].setYear(aLineInputobj_in.getYear());
				cd_in[i].setMonth(aLineInputobj_in.getMonth());
			}
		}
	}
}

package com;
/**
 * 
 */

/**
 * @author Priy Ranjan
 * 
 * This class is object representation of
 * a CSV file line (except header)
 * 
 * When CSVLineParser class parses the line using 'FillLineInput'
 * an object of this class is filled.
 *
 */
public class CSVLinePresentation
{	
	private int year;
	private String month;
	private int[] sharePrices;
	
	/**
	 * @return the sharePrices
	 */
	public int[] getSharePrices() {
		return sharePrices;
	}


	/**
	 * @param sharePrices the sharePrices to set
	 */
	public void setSharePrices(int[] sharePrices) {
		this.sharePrices = sharePrices;
	}


	/**
	 * @return the year
	 */
	public int getYear() {
		return year;
	}


	/**
	 * @param year the year to set
	 */
	public void setYear(int year) {
		this.year = year;
	}


	/**
	 * @return the month
	 */
	public String getMonth() {
		return month;
	}


	/**
	 * @param month the month to set
	 */
	public void setMonth(String month) {
		this.month = month;
	}


	/**
	 * clears the instance fields
	 */
	public void clear()
	{
		sharePrices = null;
		year = 0;
		month = null;
	}
	
	
	/**
	 * Checks if this object has default values
	 * this method helps in running test cases
	 * 
	 * @return
	 */
	public boolean hasDefaultValues()
	{
		return this.sharePrices == null && this.year == 0 && this.month == null;
	}
}

package com;
import java.io.BufferedReader;
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.IOException;

/**
 * @author Priy Ranjan
 * 
 * This class has main method and is starting point of project.
 */
class CSVProcessor
{
	/**
	 * main method
	 * 
	 * @param args
	 */
	public static void main(String[] args)
	{
		BufferedReader br = null;
		CompanyDetails[] compDetails = null;
		try
		{
			br = new BufferedReader(new FileReader("..//..//csvinput//test_shares_data.csv"));
			
			String header = br.readLine();
			if(header != null)
			{
				compDetails = CSVLineParser.fillCompanyNames(header);
				if(compDetails == null)
				{
					System.out.println("Header is missing");
					return;
				}
				
				CSVLinePresentation aCSVLinePresObj = new CSVLinePresentation();
				
				String line = null;
				while((line = br.readLine()) != null)
				{
					if(!line.trim().equals(""))
					{
						aCSVLinePresObj.clear();
						CSVLineParser.updateLinePresentation(line, aCSVLinePresObj);
						CSVLineParser.updateCompanyDetails(aCSVLinePresObj, compDetails);
					}
				}
			}
		}
		catch(FileNotFoundException fnf)
		{
			fnf.printStackTrace();
		}
		catch(IOException ioe)
		{
			ioe.printStackTrace();		
		}
		finally
		{
			if(br!=null)
			{
				try
				{
					br.close();
				}
				catch (IOException ioe)
				{
					System.out.println("Buffered reader can't be closed.");
				}
			}
		}
		
		// the company details object has the max share price and 
		// corresponding details at any time
		// just print to show output. 
		if(compDetails != null)
		{
			for(int i=0;i<compDetails.length;i++)
			{
				CompanyDetails comp = compDetails[i];
				System.out.println(comp.getCompanyName() + " " + 
								   comp.getYear() + " " + 
								   comp.getMonth() + " " + 
								   comp.getMaxSharePrice());
			}
		}
	}
}

package test;

import java.io.BufferedReader;
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.IOException;
import java.util.Scanner;

import com.CSVLineParser;
import com.CSVLinePresentation;
import com.CompanyDetails;

import junit.framework.TestCase;

/**
 * @author Priy Ranjan
 * 
 * Test class to test CSVLineParser
 */
public class Tester extends TestCase
{
	/**
	 * testcase to test if companies are added 
	 * and count is correct.
	 */
	public void test_fillCompanyNames()
	{
		Scanner in = new Scanner(System.in);
		System.out.println("Please enter the fullpath of correctly formatted csv file (test data): ");
		String filepath = in.nextLine();
		
		BufferedReader br = null;
		
		try
		{
			br = new BufferedReader(new FileReader(filepath));
			String header = br.readLine();
			CompanyDetails[] cd = CSVLineParser.fillCompanyNames(header);
			assertEquals(cd.length,5);
		}
		catch(FileNotFoundException fnf)
		{
			fnf.printStackTrace();
		}
		catch(IOException ioe)
		{
			ioe.printStackTrace();
		}
	}

	
	/**
	 * testcase to test if input csv header is ill formatted, then
	 * companies are not added
	 */
	public void test_fillCompanyNames_illFormatHeader()
	{

		Scanner in = new Scanner(System.in);
		System.out.println("Please enter the fullpath of ill- formatted header csv file (test data): ");
		String filepath = in.nextLine();
		
		BufferedReader br = null;
		
		try
		{
			br = new BufferedReader(new FileReader(filepath));
			String header = br.readLine();
			CompanyDetails[] cd = CSVLineParser.fillCompanyNames(header);
			assertEquals(cd,null);
		}
		catch(FileNotFoundException fnf)
		{
			fnf.printStackTrace();
		}
		catch(IOException ioe)
		{
			ioe.printStackTrace();
		}
	}

	
	/**
	 * tests if max share price is calculated correctly
	 */
	public void test_checkMaxSharePrice()
	{
		Scanner in = new Scanner(System.in);
		System.out.println("Please enter the fullpath of correctly formatted csv file (test data): ");
		String filepath = in.nextLine();
		
		BufferedReader br = null;
		CompanyDetails[] compDetails = null;
		
		try
		{
			br = new BufferedReader(new FileReader(filepath));
			String header = br.readLine();
			compDetails = CSVLineParser.fillCompanyNames(header);
			assertEquals(compDetails.length,5);
			
			CSVLinePresentation aCSVLinePresObj = new CSVLinePresentation();
			
			String line = null;
			while((line = br.readLine()) != null)
			{
				if(!line.trim().equals(""))
				{
					aCSVLinePresObj.clear();
					CSVLineParser.updateLinePresentation(line, aCSVLinePresObj);
					CSVLineParser.updateCompanyDetails(aCSVLinePresObj, compDetails);
				}
			}
		}
		catch(FileNotFoundException fnf)
		{
			fnf.printStackTrace();
		}
		catch(IOException ioe)
		{
			ioe.printStackTrace();
		}
		
		for(int i=0;i<compDetails.length;i++)
		{
			switch(i)
			{
				case 0: assertEquals(compDetails[i].getMaxSharePrice(),1000); break;
				case 1: assertEquals(compDetails[i].getMaxSharePrice(),986); break;
				case 2: assertEquals(compDetails[i].getMaxSharePrice(),995); break;
				case 3: assertEquals(compDetails[i].getMaxSharePrice(),999); break;
				case 4: assertEquals(compDetails[i].getMaxSharePrice(),995); break;
			}
		}
	}
}
