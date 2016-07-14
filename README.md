#include<stdio.h>
#include<stdlib.h>
#include<string.h>
int main()
{
    char *text;
    int len;
    int i=0;
    FILE *fp1;
    FILE *fp2;
    char lat[10];
    char lon[11];
    char time[21];
	fp1 = fopen("export.gpx","r");
	fp2 = fopen("export.csv","w");
	if((fp1 = fopen("export.gpx","r"))==0)
    {
        printf("文件不能正常打开");
        return (-1);
    }              
    fseek(fp1,SEEK_SET,SEEK_END);                 
    len=ftell(fp1);
    text=(char *)malloc(len);
    printf("length=%d\n",len);  
    fseek(fp1,SEEK_END,SEEK_SET);
    fread(text,sizeof(char),len,fp1);
	fprintf(fp2,"经度,,纬度,,时间\n");  
    while(!((*(text+i)=='<')&&(*(text+i+1)=='/')&&(*(text+i+2)=='g')&&(*(text+i+3)=='p')&&(*(text+i+4)=='x')&&(*(text+i+5)=='>')))
    {   
		if(((*(text+i)==' ')&&(*(text+i+1)=='l')&&(*(text+i+2)=='a')&&(*(text+i+3)=='t')))
    	{
    		strncpy(lat,&text[i+6],9);
    		strncpy(lon,&text[i+22],10);
    		strncpy(time,&text[i+44],20);
   			lat[9]='\0';
   			lon[10]='\0';
   			time[20]='\0';
			printf("%s\t",lat);
			printf("%s\t",lon);
			printf("%s\n",time);
			fprintf(fp2,"%s,,%s,,%s\n",lat,lon,time);
		}		
    	i++;		
	}
    fclose(fp1);
    fclose(fp2);
    free(text);
	return 0;
}
