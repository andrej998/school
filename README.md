#include <stdio.h>
#include <string.h>
#include <stdlib.h>

struct student{
	char index[11];
	char ime[20];
	char prezime[20];
	int brBodova;
};

int ucitajStudente(char *,struct student *);
void ispisStudenata(struct student *,int );
void trazenjePrezimena(struct student *,int );
void prekoPoena(struct student *, int , int ,char *);
void prosecnaDuzinaImena(struct student *,int );
void sortirajStudente(struct student *,int );

int main(int brArg, char *arg[]){

	struct student studenti[100];
	char s[20];
	
	sprintf(s,"preko-%s-poena",arg[2]);

	int brPoena=atoi(arg[2]);

	int brStudenata=ucitajStudente(arg[1],studenti);

	sortirajStudente(studenti,brStudenata);

	ispisStudenata(studenti,brStudenata);

	prekoPoena(studenti,brStudenata,brPoena,s);

	trazenjePrezimena(studenti, brStudenata);

	prosecnaDuzinaImena(studenti,brStudenata);

	return 0;
}

int ucitajStudente(char *nazivFajla,struct student *studenti){

	int i=0;
	FILE *file;
	file=fopen(nazivFajla, "r");
	while((fscanf(file,"%s %s %s %d",studenti[i].index,studenti[i].ime,studenti[i].prezime,&studenti[i].brBodova))!=EOF)
		i++;
	fclose(file);
	return i;
}

void sortirajStudente(struct student *studenti,int brStudenata){
	int i,broj[50],j=0,k;
	char br[3];

	while(j<brStudenata){

	br[0]='\0';
	br[1]='\0';
	br[2]='\0';
	k=0;

	for(i=2;studenti[j].index[i]!='/';i++){
		br[k]=studenti[j].index[i];
		k++;
	}

	broj[j]=atoi(br);
	j++;

	}
	
	struct student djak;
	int t;

	for(i=0;i<brStudenata-1;i++)
		for(j=i+1;j<brStudenata;j++)
			if(broj[j]<broj[i]){
				djak=studenti[i];
				studenti[i]=studenti[j];
				studenti[j]=djak;

				t=broj[i];
				broj[i]=broj[j];
				broj[j]=t;
			}
}

void ispisStudenata(struct student *studenti,int brStudenata){
	int i;
	FILE *file;
	file=fopen("izlazni","w");
	for(i=0;i<brStudenata;i++){
		fprintf(file,"%s %s %s %d\n",studenti[i].index,studenti[i].ime,studenti[i].prezime, studenti[i].brBodova);
	}
	fclose(file);
}

void trazenjePrezimena(struct student *studenti,int brStudenata){
	int i,max=0,min=100,k,j,len;
	for(i=0;i<brStudenata;i++){
		len=strlen(studenti[i].prezime);
		if(len>max){
			max=len;
			j=i;
		}
		if(len<min){
			min=len;
			k=i;
		}
	}
	
	printf("Student sa najkracim prezimenom: %s\n",studenti[k].prezime);
	printf("Student sa najduzim prezimenom: %s\n",studenti[j].prezime);
}

void prekoPoena(struct student *studenti, int brStudenata, int brPoena,char *s){
	int i;
	FILE *file;
	file=fopen(s,"w");
	for(i=0;i<brStudenata;i++){
		if(studenti[i].brBodova>brPoena)
			fprintf(file,"%s %s %s %d\n",studenti[i].index,studenti[i].ime,studenti[i].prezime,studenti[i].brBodova);
	}

	fclose(file);
}

void prosecnaDuzinaImena(struct student *studenti,int brStudenata){
	int i,duzina=0;
	for(i=0;i<brStudenata;i++){
		duzina+=strlen(studenti[i].ime);
	}
	printf("Prosecna duzina imena studenata je: %d\n", duzina/brStudenata);
}
