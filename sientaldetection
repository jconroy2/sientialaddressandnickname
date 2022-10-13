#include <stdio.h>
#include <stdlib.h>
#include <malloc.h>
#include <time.h>

//returns the date and time
char *getDateAndTime() {
    time_t t;
    time(&t);
    return ctime(&t);
}

struct address_t
{
    //component for the four integers of an IPv4 addreess
	int address[4];
	//store an associated alias of up to 10 characters
	char alias[10];
};
typedef struct address_t* node;

int countLines()
{
	//counts number of addresses present in file
	int line=0;
	FILE *fptr;
	char chunk[128];

	//opens file
	fptr=fopen("CS222_Inet.txt","r");
	if(fptr==NULL)
	{
		printf("File \"CS222_Inet.txt\" does not exists.");
		return(NULL);
	}

	//read until line is present
	 while(fgets(chunk, sizeof(chunk), fptr) != NULL)
	 {
    	if(strcmp(chunk,"0.0.0.0 NONE\n")==0)
    	{
    		return line;
    	}
		line+=1;
     }

    //closes the file pointer
    fclose(fptr);

    //returns number of lines present
    return line;
}

int readfiles(struct address_t *address_t_array,int valid_lines)
{
	FILE *fptr;
	FILE *error_file_fptr;
    size_t len = 0;
	char chunk[128];
	char str1[128];

	//opens the file CS222_Inet.txt
	fptr=fopen("CS222_Inet.txt","r");
    error_file_fptr=fopen("CS222_Error_Report.txt","w");
	int i;
	int line=0;

	//reads the file until line in file are present and line number less than valid lines
	 while(fgets(chunk, sizeof(chunk), fptr) != NULL && line<valid_lines) {
	 	strcpy(str1,chunk);
		char *token = strtok(chunk," .");
		    i=0;
		    while (token != NULL)
		    {
		    	//for storing the alias name as string
		    	if(i==4)
		    		memcpy(address_t_array[line].alias,token,sizeof(token));

		    	//stores each component of address as integer
		    	else
		    	{
			    	int comp=atoi(token);

			    	//if the address component is in valid range
			    	if(comp>=0 && comp<=255)
						address_t_array[line].address[i]=comp;

					//otherwise the address is added to CS222_Error_Report
					else
					{
						fprintf(error_file_fptr,"%s\n",str1);
					    line-=1;
					    valid_lines-=1;
					    break;
					}
		    	}

		        token = strtok(NULL, " .");
		        i=i+1;
		    }

		    line+=1;
     }
     fclose(error_file_fptr);
     fclose(fptr);

     //returns number of valid lines
     return line;
}

void generateLocalityRpt(char name[],struct address_t *address_t_array,int line)
{
	FILE *fptr;
	int i=0,exists=0,j;
	int localityLength=0;
	int locality[line][2];

	//storing unique address in array 'locality'
	while(i<line)
 	{
 		exists=0;
 		j=0;
 		while(j<=(localityLength-1))
 		{
 			//if the the locality already exists in locality array set exists=1
 			if(locality[j][0]==address_t_array[i].address[0] && locality[j][1]==address_t_array[i].address[1])
 			{
 				exists=1;
 				break;
			}
			j+=1;
		 }

		 //add the new locality to 'locality' array only if does not already exists in locality array
		if(exists==0)
		{
			locality[localityLength][0]=address_t_array[i].address[0];
			locality[localityLength][1]=address_t_array[i].address[1];
			localityLength+=1;
		}

		i+=1;
	 }
	j=0;
    fptr=fopen("222_Locality_Report.txt","w");
    //writes name and date
    fprintf(fptr,"%s %s",name,getDateAndTime());
    fprintf(fptr,"CS222 Network Locality Report\n");
    fprintf(fptr,"Address total:%d\n",line);
    fprintf(fptr,"Localities:%d\n",localityLength);
    while(j<localityLength)
    {

    	i=0;
    	fprintf(fptr,"\n%d.%d\n",locality[j][0],locality[j][1]);
    	while(i<line)
    	{
    		if(locality[j][0]==address_t_array[i].address[0] && locality[j][1]==address_t_array[i].address[1])
    		{
    			fprintf(fptr,"%s",address_t_array[i].alias);
			}
		i+=1;
		}

    	j+=1;
	}
	fclose(fptr);
}
int main()
{
    char name[50];
    //counts the number of line in file
    int line=countLines();

    //if file does not exists then return
    if(line==NULL)
    	return;
    //declaring variable to store different addresses
    int locality[line][2];

    //allocating memory to store all the addresses
    struct address_t *address_t_array=(struct address_t*)malloc(line*sizeof(struct address_t));

    //reading the addresses
    line=readfiles(address_t_array,line);
 	printf("Enter your name: ");
 	scanf("%s",&name);
 	generateLocalityRpt(name,address_t_array,line);
 	printf("\nFiles \"222_Locality_Report.txt\" and \"CS222_Error_Report\" created successfully.\n");
    return 0;
}
