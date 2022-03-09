# VENTA
 //TRABAJO INDIVIDUAL DEL PROYECTO
#include <stdio.h>
#include <stdlib.h>
#define N 100
typedef struct{
	int nro;
	char genero;// H es hombre, M mujer
	char seccion [50];//nino, adulto,infante
	int talla;//32-40
	int cantidad;
	float precio; //precio de venta
	float descuento; //0 no tiene descuento
	int estado;
}zapato;
typedef struct{
	int nro;
	char genero;// H es hombre, M mujer
	char calidad[30];//cuero, jean, tela, cuerina
	char marca[30];
	char tamanio;//p(pequeño),M(mediano),G(Grande)
	int cantidad;
	float precio;// precio d venta
	float descuento;//0 no tiene descuento, 0.3
	int estado;
}cartera;
typedef struct{
	//fecha fechaNac;
	char cedula[11];
	char nombres[50];
	int edad;//debe ser calculado
	char celular[10];
}cliente;
typedef struct{
	int nro;
	zapato zapatov; //Datos quemados(anidados)
	cliente clientev; //Datos quemados(anidados)
	int cantidad;
	float subtotal;
	float iva;
	float descuento;
	float total;
	int estado;	
}venta;//ALCIVAR ZAMBRANO GRIZZLY

//OBJETIVOS:
//- PRESENTAR EL VALOR TOTAL A PAGAR por los zapatos
//- EL DESCUENTO APLICADA a los zapatos
//- ELIMINAR VENTA de zapatos
//- PRESENTAR VENTA  de zapatos

//DECLARACION DE FUNCIONES
void presentar();
void registrar(int i, int nro);
int buscarventa();
void verregistro(int pos);
void verDatos(int j);
int leernumero(char mensaje[100], int min, int max);
//DECLARACION DE VARIABLES
int n=0;
venta *ventas=NULL; //arreglo de ventas 
int main(){
	int nro=1, i=0, pos=-1;
	char op='0';
	char mensaje[]="\nVENTA DE ZAPATOS \n[1] Registar su compra en zapatos \n[2] Presentar su compra\n[3] Eliminar compra \n[0] salir \nelija su opcion ";
	printf("cuantas compras de zapatos realizo hoy? ");scanf("%d",&n);fflush(stdin);
	ventas=(venta *)malloc(n*sizeof(venta)); //arreglo dinamico, solo se crean n espacios de memoria y ya no N
	do{
		printf(mensaje);scanf("%c",&op);fflush(stdin);
		switch(op){
			case '1': if (i<n){
				registrar(i,nro);
				nro++;
				}
			    i++;
			break;
			case '2': presentar();
			break;
			case '3': //eliminar venta de productos (busqueda )
				pos=buscarventa();
				if(pos!=-1){
					verregistro(pos);
				}else
					printf("venta no encontrada");
			break;
		}
	}while(op!='0');
	free(ventas);
	return 0;	
}
void verregistro(int pos){
	char resp= ' ';
	verDatos(pos);
	printf("\nDesea eliminar la venta (s/n))");scanf("%c",&resp);fflush(stdin);
	if(resp=='S'||resp=='s')
		ventas[pos].estado=0;
}
int buscarventa(){
	int pos=0, j=0, nroB=0,bandera=0;
	venta aux;
	printf("\ningrese el numero de factura a buscar");
	scanf("%d",&nroB);fflush(stdin);
	bandera=0;
	for(j=0;j<n&&bandera==0;j++){
		if(ventas[j].nro==nroB&&ventas[j].estado==1){
			aux=ventas[j];
			pos=j;
			bandera=1;
		}
	}
	return pos;
}
void presentar(){
	int j=0;
	printf("\nLas ventas de zapatos registradas son: ");
	for(j=0;j<n;j++){
		if(ventas[j].estado==1)
			verDatos(j);
	}
}
void verDatos(int j){
	printf("\nNro: %d",ventas[j].nro);
	printf("\nCliente: %s",ventas[j].clientev.nombres);
	printf("\nCedula: %s",ventas[j].clientev.cedula);
	printf("\nCantidad: %d",ventas[j].cantidad);
	printf("\nSubtotal: $ %.2f",ventas[j].subtotal);
	printf("\nDescuento:  %.2f %",ventas[j].descuento);
	printf("\nIVA: $ %.2f",ventas[j].iva);
	printf("\nTotal: $ %.2f",ventas[j].total);
	printf("\n");
}
void registrar(int i,int nro){
	cliente clientes[]={{"599229119"," Ana Lopez",18,"099229113"},{"4989140552","Andres Q.",18,"098910552"},{"4674540552","Karla H.",18,"09898552"}};
	zapato zapatos[]={{1,'M',"nino/adulto/infante",32,30,40.0,40.0,1},{2,'H',"nino/adulto/infante",40,50,30.0,30.0,1}};
	int x=0, y=0,c=0;
	x=leernumero("\n[1]comprador 1\n[2]comprador 2\n[3]comprador 3 \n elija ",1,3);
	y=leernumero("\n[1]Zapatos de mujer $40 \n[2]zapatos de hombre $30 \n elija: ",1,2);
	printf("cuantos pares de zapatos quiere? ");scanf("%d",&c);fflush(stdin);
	ventas[i].nro=nro;
	ventas[i].cantidad=c;
    ventas[i].clientev=clientes[x-1];
	ventas[i].subtotal=c*zapatos[y-1].precio;
	ventas[i].iva=ventas[i].subtotal*0.12;
	ventas[i].descuento=c*0.3;
	ventas[i].total=(ventas[i].subtotal*ventas[i].iva)-ventas[i].descuento;
	ventas[i].estado=1;
}
int leernumero(char mensaje[100],int min,int max){
	int x=0;
	do{
		printf(mensaje);scanf("%d",&x);fflush(stdin);
	}while(x<min||x>max);
	return x;
}
