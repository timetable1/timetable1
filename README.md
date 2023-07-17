/*2. Write a program to read and write student objects with fixed-length records and the
fields delimited by “|”. Implement pack ( ), unpack ( ), modify ( ) and search ( )
methods.*/

#include<stdio.h>
#include<conio.h>
#include<fstream.h>
#include<string.h>
#include<process.h>
 
 void insert();
 void pack();
 void unpack();
 void help();
 void modify();
 void search();

 class student
   {
 public:
	char name[25],age[5],sem[5],branch[5],usn[5];
   };
 student s;
 char buffer[55],temp[55];
 fstream fp;
 fstream gp;

 void main()
  {  
    int c;
	fp.open("fixed.txt",ios::app);        //creating file fixed.txt
	fp.close();
      clrscr();
    while(1)
	{
		cout<<"\t*****Enter your choice*****\n";
		cout<<"1.Pack 2.Unpack 3.Modify 4.Search 5.Exit"<<endl;
		cin>>c;

		switch(c)
	       {
		case 1:pack();
		break;
		case 2:unpack();
		break;
		case 3:modify();
		break;
	      case 4:search();
		break;
		case 5:exit(0);
		break;
		default:cout<<"Invalid input\n";
		break;
	    }
	}
 }

 void pack()
   {
	fp.open("fixed.txt",ios::app);
	insert();
	fp<<buffer<<endl //inserting the data into the file from the buffer
	cout<<"\t*****Record insertion successful*****"<<endl; 
                                     //using file pointer fp.
	fp.close();
	getch();
	clrscr();
   }
 void insert()
   {
	cout<<"USN";
	cin>>temp;
	strcpy(buffer,temp);
	strcat(buffer,"|");
	cout<<"Name";help();
	cout<<"sem";help();
	cout<<"age";help();
	cout<<"Branch";help();
	int c=strlen(buffer);
	for(int i=0;i<55-c;i++)       //Inserting filling character "!"
	strcat(buffer,"!");
   }
void help()
    {
	cin>>temp;
	strcat(buffer,temp);
	strcat(buffer,"|");
    }

void unpack()
 {
	fp.open("fixed.txt",ios::in);
	fp.getline(buffer,60,'\n'); 
                       //fp.getline:To get the first record from the file

	cout<<"\t*****Unpacked record*****"<<endl;
	cout<<"USN\t\tName\tSEM\tage\tBranch"<<endl;
	while(!fp.eof())                             
                      //execute till fp reaches end of file.
	 {
	  sscanf(buffer,"%[^|]|%[^|]|%[^|]|%[^|]|%[^|]|",s.usn,s.name,s.sem,s.age,s.branch);
	  cout<<s.usn<<"\t"<<s.name<<"\t"<<s.sem<<"\t"<<s.age<<"\t"<<s.branch<<"\n";
	  fp.getline(buffer,60,'\n');
	 }
    fp.close(); //close the opened file.otherwise U'll get infinite loop.
    getch();
    clrscr();
 }

void modify()
  {
	char key[25],usn[25],modify[55];
	int Notfound=1;				              	//Notfound:status flag.
	cout<<"Enter the key to Modify (USN)\n";
	cin>>key;
    fp.open("fixed.txt",ios::in);
    gp.open("fixed1.txt",ios::app);

    fp.getline(buffer,60,'\n');
    while(!fp.eof())
     {
	sscanf(buffer,"%[^|]|",usn);
//scan the buffer to get the USN of the record.
	if(strcmp(usn,key)!=0) 				        
//if key not found,simply copy the buffer content to
	    {					                     	//the new file.
		gp<<buffer<<endl;
	    }
	else						               //if found 
		{
		Notfound=0;
		cout<<"\t*****record found*****"<<endl;
		cout<<"Enter New data"<<endl;
		insert();				              //take new data.
		strcpy(modify,buffer); 
                           //copy new data into the array "modify".
		gp<<modify<<endl;				        
                          //insert new data to the new file.
		cout<<"\t*****Record Modification successful*****"<<endl;
		}
		fp.getline(buffer,60,'\n');

	}
	if(Notfound)						
                         //if Notfound=1;record not found.
		{
		  cout<<"\tRecord not found  :("<<endl;
		}
  fp.close();							
//close both the file pointers before the rename,or remove operations.
  gp.close();
  char new1[]={"fixed1.txt"};		
  char old[]={"fixed.txt"};
  remove("fixed.txt");						
//Before rename you must remove the old file,otherwise rename 
  rename(new1,old);	
//operation will fail.
  getch();
  clrscr();
}
void search()
   {
	char key[25],usn[25],modify[55];
	cout<<"enter the key to search (USN)\n";
	cin>>key;

	fp.open("fixed.txt",ios::in); 
	fp.getline(buffer,60,'\n');
  
     while(!fp.eof())
	  {
		sscanf(buffer,"%[^|]|",usn);
		if(strcmp(usn,key)!=0);				
//if key not matched with USN,Do nothing
		else
		  {
		  cout<<"\t*****record found*****"<<endl;//if found print it.
		  cout<<"USN\t\tNAME\tSEM\tAge\tBranch"<<endl;
		  sscanf(buffer,"%[^|]|%[^|]|%[^|]|%[^|]|%[^|]|",s.usn,s.name,s.sem,s.age,s.branch); //copy the buffer contents to the
		  cout<<s.usn<<"\t"<<s.name<<"\t"<<s.sem<<"\t"<<s.age<<"\t"<<s.branch<<"\n";	    
 //variables,excluding "|" symbol.
		 
		  break;				
//After finding the record,BREAK the loop.No need of further search.			
		  }
	      fp.getline(buffer,60,'\n');
		  if(fp.eof())			
//while searching if fp reaches eof;means that no record matched wid key
	   	   {				//so record not found.
		  cout<<"\tRecord not found  :("<<endl;
		   }
	  }
	      fp.close();
	      getch();
	      clrscr();
   }
