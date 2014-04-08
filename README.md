JocParelles
===========
#include <stdio.h>
#include<conio.h>
#include<stdlib.h>

int menu (void);
void mostrar (int,char*);
void plenar (int,char*);
int joco (char*,char*,int);
int plena(char*,int);
void iguals(char*,char*,int,int);

int main(void)
{
	char taula8[8][8],taula6[6][6],taula4[4][4],taulamostra[8][8],*punter,*punterprincipal;
	int dimen,x,vegades=0;
    
    punter=&taulamostra[0][0];//marques on apunta el punter
    for(x=0;x<64;x++)//buides tota la matriu auxiliar
	{
		*(punter+x)=219;
	}
    dimen=menu();
    /*depenen de quina opcio marquis al menu el punter apuntara a una posicio
    o a una altra*/
    if(dimen==4)punterprincipal=&taula4[0][0];
    else if(dimen==6)punterprincipal=&taula6[0][0];
    else if(dimen==8)punterprincipal=&taula8[0][0];
    plenar(dimen,punterprincipal);//plena la matriu amb els caracters que jugarem
    //Es on es desenvolupe tot el joc
    vegades=joco(punterprincipal,punter,dimen);
    //Mostra les vegades jugades i el % d'acert realitzat
    printf("Has jugat %d vegades amb un %.2f de encert\n", vegades, ((dimen*dimen/2.0)/vegades)*100.0);
    getche();
    return(0);
    
}
/*Entra un punter que sera la taula on estan tots els caracters guardats, un altre punter
que marca la matriu que esta tota buida, i finalment un int que es el numero de la
dimensio, surt un int que contara el numero de vegades que jugues*/
int joco (char *punter,char *sub,int dimen)
{
    int cont=0,col1,fila1,col2,fila2,num1,num2;
    char buit=219;
    //Mentres no estigui plena seguira jugant
    while((plena(sub,dimen))!=0)
    {
    						  mostrar(dimen,sub);
                              printf("\nEntra les posicions que vols emparellar:\n");
                              printf("Fila: ");
                              scanf("%d",&fila1);
                              printf("\nColumna: ");
                              scanf("%d",&col1);
                              //Operacio per a calcular la posicio que el usuari demana
                              num1=(fila1*dimen)+col1;
                              //Comprovacio d'errors d'entrada de posicio
                              while(*(sub+num1)!=buit || (fila1)>dimen || (fila1)<0 || (col1)>dimen || (col1)<0)
                              {
                              		printf("Entra una posicio correcta.\n");
							  		printf("Fila: ");
                              		scanf("%d",&fila1);
                              		printf("\nColumna: ");
                              		scanf("%d",&col1);
                              		num1=(fila1*dimen)+col1;
                              }
                              printf("Fila: ");
                              scanf("%d",&fila2);
                              printf("\nColumna: ");
                              scanf("%d",&col2);
                              //Operacio per a calcular la posicio que el usuari demana
                              num2=(fila2*dimen)+col2;
                              //Comprovacio d'errors d'entrada de posicio
                              while(*(sub+num2)!=buit || (fila2)>dimen || (fila2)<0 || (col2)>dimen || (col2)<0 || num2==num1)
                              {
                              		printf("Entra una posicio correcta.\n");
							  		printf("Fila: ");
                              		scanf("%d",&fila2);
                              		printf("\nColumna: ");
                              		scanf("%d",&col2);
                              		num2=(fila2*dimen)+col2;
                              }
                              //Guarda el valor de les posicions a la taula auxiliar
							  *(sub+num1)=*(punter+num1);
                              *(sub+num2)=*(punter+num2);
                              //Mostra les posicions entrades
                              mostrar(dimen,sub);
                              /*S'encarregue de mirar si el contigut es igual o no
                              i si no ho es el torna a possar en blanc*/
                              iguals(punter,sub,num1,num2);
                              //Contador de les rondes jugades
                              cont++;
                              getche();
                              //Llimpie la pantalla
                              system("cls");
							  printf("\n"); 
                              
                              
    }
    return(cont);
}
/*es un void que simplement retornara un int que sera la opcio/diemnsio amb la
que es fara el joc*/
int menu(void)
{
	int x=0; 
	
	printf("Quina dimencio vols la matriu del joc:\n1-4x4.\n2-6x6.\n3-8x8.\n");
	while(x<1 || x>3)scanf("%i",&x);
	//simplement pase l'opcio seleccionada amb una dimensio
	if(x==1)x=4;
	else if(x==2)x=6;
	else if(x==3)x=8;
	
	return(x);
}
/*Entra un int que sera la dimensio de la taula amb la que volem mostrar i un int
que es la diemnsio de la taula*/
void mostrar(int dimen, char *taula)
{
	int x=0;
	
	for(x=0;x<(dimen*dimen);x++)
	{
		printf("%c\t",*(taula+x));
		//Serveix per fer un intro al finalitzar cada fila
		if(dimen==4 && (x==3 || x==7 || x==11))printf("\n");
		else if(dimen==6 && (x==5 || x==11 || x==17 || x==23 || x==29))printf("\n");
		else if(dimen==8 && (x==7 || x==15 || x==23 || x==31 || x==39 || x==47 || x==55))printf("\n");
	}
	
}
/*Entre un int que es la dimensio de la taula i un punter que sera la taula que
volem plenar dels caracters amb els que jugarem*/
void plenar(int dimen, char *taula)
{
	char valor='A',valor2='1';
	int x=0,aux=0,posicio1,posicio2;
	
	srand(time(NULL));
	//inicialitzes tot a buit
	for(x=0;x<(dimen*dimen);x++)
	{
		*(taula+x)=' ';
	}
	x=0;
    
	while((dimen*dimen/2)>x)
	{
        //Creo 2 posicions random
		posicio1=rand()%(dimen*dimen);
		posicio2=rand()%(dimen*dimen);
		//miro que les dos estiguin buides sino es torna a repetir
		while(*(taula+posicio1)!=' ')posicio1=rand()%(dimen*dimen);
		/*Mentres el caracters no s'acabin va augmentan el valor i el va guardant
		si els caracters s'acaben pasara a guardar numeros en format char*/
		if(x<=25)*(taula+posicio1)=(valor+x);
		if(x>25)*(taula+posicio1)=(valor2+aux);
		
		while(*(taula+posicio2)!=' ')posicio2=rand()%(dimen*dimen);
		if(x<=25)*(taula+posicio2)=(valor+x);
		if(x>25)*(taula+posicio2)=(valor2+aux);
		if(x>25)aux++;
		x++;	
		
	
	}
}
/*Funcio que serveix per a saber si una matriu esta plena o no mitjançant la entrada
de un punter que es la matriu que mirarem i un int que es la dimensio de la matriu*/
int plena(char *matriu,int dimen)
{
    int x,final=0;
    char buit=219;
    //Busca a les posicions si son igual a buit i suma a la variable final
    for(x=0;x<(dimen*dimen);x++)
    {
                if(*(matriu+x)==buit)final++;
    }
    //Si troba una posicio que esta buida retorna un 0 
    if(final>0)final=1;
    //Si no troba cap buida retornara el valor final=0
    return(final);
}
/*Es la funcio que sencarregue de mirar si dos posicions de les matrius son iguals
o no, entre dos punters que son les dos matrius amb les que juguem, una es la plena
i la altra la que utilitzem per a anar guardant les dades, i dos int que son el
valor de les 2 posicions amb les que jugarem*/
void iguals(char *punter,char *sub,int num1,int num2)
{
    //Si el valor que contenen es diferent els torna a possar buit
	if(*(punter+num1)!=*(punter+num2))
    {
                  *(sub+num1)=219;
                  *(sub+num2)=219;
                  printf("\nNo has trobat una parella\n");
    }
    //Si son iguals els deixa com estaen
    else printf("\nHas trobat una parella\n");
}
