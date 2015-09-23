import java.io.*;
import java.util.*;

class DAA 
{
 
public static void main(String args[]) throws IOException
	{
            
			//n number of programs and number of memory units
			InputStreamReader isr =  new InputStreamReader(System.in);
			BufferedReader br = new BufferedReader(isr);

			String input=br.readLine();
			
			StringTokenizer st=new StringTokenizer(input," ");
			st.hasMoreTokens();
			String m=st.nextToken(); //m=no of memeory regions ,in= no of programs
			String in=st.nextToken();
			
			int mm = Integer.parseInt(m);
			int inin=Integer.parseInt(in);
			int case_=0;
			
			if((0>mm)||(mm >11))
			{
				System.out.println("Error : Number of memory region should be between 1 and 10");
			}
			
			else if((0>inin) || (inin>51))
			{
				System.out.println("Error : Number of programs should be between 1 and 50");
			}
			
			else 
			{
				do
				{
					case_ =case_+1;
					
					//# of regions
					int storage = Integer.parseInt(m);
			
					//# of programs
					int n = Integer.parseInt(in);
			
					//memory partion sizes initialize
					int[] n_storage =new int[storage];  
					String linex=br.readLine();
					StringTokenizer stline = new StringTokenizer(linex," ");
			
					//--Storage partion sizes
					for(int x=1;x<=storage;x++)
					{
						n_storage[x-1]=Integer.parseInt(stline.nextToken());
					}
					//order array initialize
					int[] order=new int[n];
					int[] pSize= new int[n];
					int[] eTime= new int[n];
    
					for(int i=1;i<=n;i++)
					{
						String line=br.readLine();
						StringTokenizer st2=new StringTokenizer(line, " ");
				
						//save to array by order    
						order[i-1]=i;
						int pairs=Integer.parseInt(st2.nextToken());
				
						int[] time_= new int[pairs];
						int[] size_= new int[pairs];
						
						//for pairs
						for(int k=1;k<=pairs;k++)
						{
                    
							size_[k-1]=Integer.parseInt(st2.nextToken());                   
							time_[k-1]=Integer.parseInt(st2.nextToken());
						}
                
						int min=0;
                
						for(int y=1;y<=pairs;y++)
						{     
							int x=y-1;
							if( time_[x] < time_[min] )
							{
								min = x;
							}
						}
                
						//program sizes
						pSize[i-1]=size_[min];
						//execution times
						eTime[i-1]=time_[min];
                
					}
					//--array to get the time values to calculate the return time
					int[] total_time_storage =new int[storage];
                
					//return time initialization
					int returntime = 0;

					//String array to store the output line by line as array elements
					String[] msg = new String[n];
        
					int min_index = 0;
				
					for (int x = 0;x < n;x++)
					{
						min_index = x;
                        
						//Find the index of the minimum value of the programs one by one
						for (int y = x;y < n;y++)
						{
							if (eTime[y] < eTime[min_index])
							{
								min_index = y;
							}
						}
						//Sort the array values in order of the programs
						//time sorting
						int temp = eTime[min_index];
						eTime[min_index] = eTime[x];
						eTime[x] = temp;
						//program sizes sorting
						temp = pSize[min_index];
						pSize[min_index] = pSize[x];
						pSize[x] = temp;
						//order of the programs sorting
						temp = order[min_index];
						order[min_index] = order[x];
						order[x] = temp;
					}

					//Assign the storage unit for the programs
					int current_storage = 0;
					for (int x = 0;x < n;x++)
						{
							//check for the free storage to run
							for (int y = current_storage;y <= storage;y++)
							{
								if (storage == y)
								{
									current_storage = 0;
									y = 0;
								}
							else
								{
									current_storage = y;
								}
							//Assign the msg array elements 
				
								if (pSize[x] <= n_storage[y])
								{
									int regionNo=y+1;
									int totTimeStorage=total_time_storage[y];
									int totTime=total_time_storage[y] + eTime[x];
						
									msg[order[x] - 1] = "Program " +order[x]+ " runs in region " + regionNo + " from " +totTimeStorage + " to " +totTime;
									total_time_storage[y] = total_time_storage[y] + eTime[x];
									returntime = returntime + total_time_storage[y];
						
									//reset storage
									if (storage == current_storage)
									{
										current_storage = 0;
									}
									//increase the running storage
									else
									{
										current_storage++;
									}
									break;
								}
							}

						}

					//Calculate turnaround time
					System.out.println("Case: "+case_);
					System.out.print("Avg turn around time is : " + returntime /(double)n+"\n");
					//print msg array which contains the program running times and storage units
					for (int x = 0;x < n;x++)
					{
						System.out.println(msg[x]);
					}              
					//update n and storage values for the next case
				
					String linenext=br.readLine();
					StringTokenizer ss=new StringTokenizer(linenext, " ");
					m=ss.nextToken();
					in=ss.nextToken();   
				}	 while(!(("0".equals(in))&&("0".equals(m))));
			}
	}
	
}

	
 
