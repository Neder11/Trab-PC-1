#define _CRT_SECURE_NO_WARNINGS
#include <locale.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_COMB 50000
#define MAX_STRINGS 100
#define NUM_RESIST_INICIAL 12

//TRABALHO 1 "Associação de Resistências" | Andrea Cassanda Nº49129

float Combinacoes [MAX_COMB] = { 100, 200, 330, 470, 680, 1000, 2000, 3300, 4700, 6800, 33000, 100000 };
char  vstring [MAX_COMB][MAX_STRINGS] = { "100", "200", "330", "470", "680", "1000", "2000", "3300", "4700", "6800","33000", "100000"};

//Para combinações de duas resistências:

int main()
{
	setlocale(LC_ALL, "Portuguese");

	int i, j, r;
	float Serie, Paralelo, t, Imp, Tol_soma, Tol_sub;

	r = NUM_RESIST_INICIAL;

	printf("Valores das resistências iniciais (em Ohms): 100, 200, 330, 470, 680, 1000, 2000, 3300, 4700, 6800, 33000, 100000\n\n");

	printf("Valores das associações de 2 resistências (Em série e em Paralelo): \n");
	for (i = 0; i < NUM_RESIST_INICIAL; i++) {
		for (j = i; j < NUM_RESIST_INICIAL; j++) // Sendo j=i, não há valores repetidos!
		{
			Serie = Combinacoes[i] + Combinacoes[j];
			Paralelo = 1.0 / (1.0 / Combinacoes[i] + 1.0 / Combinacoes[j]);

			sprintf(vstring[r], "%s + %s", vstring[i], vstring[j]);
			Combinacoes[r] = Serie;
			printf("R(Ohm) = %.2f = %s\n", Combinacoes[r], vstring[r]); 
			r++;

			sprintf(vstring[r], "%s || %s", vstring[i], vstring[j]);
			Combinacoes[r] = Paralelo;
			printf("R(Ohm) = %.2f = %s\n", Combinacoes[r], vstring[r]);
			r++;
			if (r % 2 == 0) printf("\n");
		}
	}
	printf("Número de Combinações= %d\n", r); system("Pause");

	// Para combinações de três e quatro resistências:

	printf("\nValores das associações de 3 e 4 resistências (Mistas): \n");

	/*int lim =r;
	for (i = 0; i < lim; i++) {
		for (j = i; j < lim; j++) {

			Serie = Combinacoes[i] + Combinacoes[j];
			Paralelo = 1.0 / (1.0 / Combinacoes[i] + 1.0 / Combinacoes[j]);

			sprintf(vstring[r], "(%s) + (%s)", vstring[i], vstring[j]); 
			Combinacoes[r] = Serie;
			printf("R(Ohm) = %.2f = %s\n", Combinacoes[r], vstring[r]); 
			r++;
			
			Combinacoes[r] = Paralelo;
			sprintf(vstring[r], "(%s) || (%s)", vstring[i], vstring[j]); 
			printf("R(Ohm) = %.2f = %s\n", Combinacoes[r], vstring[r]);
			r++;
			if (r % 2 == 0) printf("\n");
		}
	} printf("Número de Combinações= %d\n", r); system("Pause");*/
	
	// Ponto 2 e 3- Tolerância
	
	t = 0.1;
	float Resis[NUM_RESIST_INICIAL] = {100, 200, 330, 470, 680, 1000, 2000, 3300, 4700, 6800, 33000, 100000};
	
	do {
		printf("Por favor, introduza um valor de tolerância compreendido entre 0,1%% e 5%%:\n");
		scanf("%f", &t); 

		if (t > 5 && t < 0.1) {
			printf("Valor inválido\n");
		}
		else {
			printf("Por favor, introduza o valor de impedância que procura: \n");
			scanf("%f", &Imp);

			Tol_soma = Imp + (t / 100);
			Tol_sub = Imp - (t / 100);

			printf("Valores de Impedância com tolerância: entre %.2f e %.2f\n", Tol_sub, Tol_soma);
			printf("Forma de calcular as Impedâncias dentro do seu intervalo a partir dos valores iniciais: \n");

			for (i = 0; i < r; i++) {

				Combinacoes[r] = Serie;
				Combinacoes[r] = Paralelo;

				if (Resis[i] >= Tol_sub && Resis[i] <= Tol_soma) {
					printf("R(Ohm)= %.2f= %.2s\n", Combinacoes[r], vstring[r]);
				}
			}
		}
		
	} while (Imp != 0);
	
	return 0;
}



