#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#define MAX_LONG_TEL 10    /* Longitud maxima del Telefono */
#define MAXLEN 80          /* Longitud maxima de las cadenas que leemos */

typedef struct
 {
   char *nombre;
   char *apellido;
   char tef[MAX_LONG_TEL];
   char *mail;
   int edad;
 } PERSONA;

PERSONA persona, *pPersona=NULL;
typedef enum { OK = 1, ERR = 0} status;


status LeerDatos(PERSONA *pp);
void   MostrarDatos(PERSONA *);
status EsDigito(char *caracter);
status CadenaVacia(char caracter[]);
int    PasaAEntero(char caracter[], int maximo);
status LiberarPersona(PERSONA *);

int main(void)
{
	
 
  pPersona = &persona;

  if (LeerDatos(pPersona) == ERR)
   {
    LiberarPersona(pPersona);
    return ERR;
   }

  MostrarDatos(pPersona);
 LiberarPersona(pPersona);

  return OK;
}

status LeerDatos(PERSONA *pp)
{
  #define LONG_TEL 10   /* Numero de digitos que debe tener el Telefono */
  #define LONG_CADENA 60
  char temp[MAXLEN];
  int i;

  printf("Introduzca los datos de la persona:\n");

  printf("\nNombre: ");
  gets(temp);
  pp->nombre = (char *) malloc((strlen(temp) + 1) * sizeof(char));
  if (CadenaVacia(temp) == OK)
    return ERR;
  else if (strlen(temp) > LONG_CADENA)
   {
    printf("La longitud de la cadena es demasiado larga");
    return ERR;
   }
  else
    strcpy(pp->nombre, temp);

  fflush(stdout);
  printf("Apellido: ");
  gets(temp);
  pp->apellido = (char *) malloc((strlen(temp) + 1) * sizeof(char));
  if (CadenaVacia(temp) == OK)
    return ERR;
   else
     strcpy(pp->apellido, temp);

  for (i=0; i<strlen(pp->apellido); i++)
   {
    if (pp->apellido[i] == ' ')
     {
       printf("Introduzca un apellido unicamente y no deje espacios.");
      return ERR;
      }
   }

  printf("Telefono (10 digitos, comience con 0 si es necesario): ");
  gets(temp);
  if (strlen(temp) != LONG_TEL)
   {
     printf("El Telefono debe tener 10 digitos.\n");

     return ERR;
    }
   if (EsDigito(temp) == OK)
    {
     for (i=0; i<strlen(temp); i++)
       pp->tef[i] = temp[i];
    }
   else
     return ERR;
 
  printf("\nMail: ");
  gets(temp);
  pp->mail = (char *) malloc((strlen(temp) + 1) * sizeof(char));
  if (CadenaVacia(temp) == OK)
    return ERR;
  else if (strlen(temp) > LONG_CADENA)
   {
    printf("La longitud de la cadena es demasiado larga");
    return ERR;
   }
  else
    strcpy(pp->mail, temp);

  fflush(stdout);

   printf("Edad: ");
   gets(temp);
   if (EsDigito(temp) == ERR)
      return ERR;
   pp->edad=PasaAEntero(temp, 2);
   if (pp->edad == ERR)
    {
     printf("\nError al intentar leer la edad.");
     printf("\nLa edad debe estar entre 1 y 99.\n");
     return ERR;
    }

  return OK;
}

void MostrarDatos(PERSONA *pp)
{
  printf("\nLos datos obtenidos son:\n");

  printf("\nNombre: %s", pp->nombre);
  printf("\nApellido: %s", pp->apellido);
  printf("\nTelefono: %s", pp->tef);
  printf("\nEdad: %d", pp->edad);
  printf("\nMail: %s", pp->mail);
}



status EsDigito(char *caracter)
{
 int i;

 CadenaVacia(caracter);

 for (i = 0; i < (int) strlen(caracter); i++)
  if (caracter[i] < '0' || caracter[i] > '9')
   {
    printf("\nIntroduzca unicamente digitos, por favor.\n");
    return ERR;
   }

 return OK;
}


status CadenaVacia(char caracter[])
{

  if (caracter[0] == 0)
   {
    printf("\nNo puede dejarlo en blanco, debe introducir los datos\n");
    return OK;
   }

  return ERR;
}

int PasaAEntero(char caracter[], int maximo)
{
 int num=0;
 int i;

 if (strlen(caracter) > maximo)
  {
   printf("\nDato erroneo.");
   return ERR;
  }


 if (strlen(caracter) == 1)
   return num = caracter[0] - '0';  

 for (i = 0; i != (int) strlen(caracter); i++)
   num = (num * 10) + (caracter[i] - '0');  

 return num;
}
status LiberarPersona(PERSONA *pp)
{

 free(pp->nombre);
 free(pp->apellido);

 return OK;
}


