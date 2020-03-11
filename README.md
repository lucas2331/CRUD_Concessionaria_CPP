<h2 align="center">CRUD Concessionaria | C++</h2>
# CRUD Concessionaria | C++

```c++

#include <stdio.h>
#include <stdlib.h>
#include <malloc.h>
#include <locale.h>
#include <iostream>

#define TRUE 1
#define FALSE 0

using namespace std;

struct Carro{
	int Codigo;
	char Nome[100];
	char Ano[100];
	char Combustivel[100];
	char Configuracao[100];
	char Lugares[100];
	char Tracao[100];
};

FILE *ArquivoConcessionaria = NULL;

/** ARQUIVO */
void criar_arquivo();
void fechar_arquivo();

/** INTERFACE */
void menu();
void executar(char tecla);

/** CRUD */
void inserir();
void pesquisar();

int main(){

  /** Aceitar Acentos **/
  setlocale(LC_ALL, "Portuguese");
  
  /** Chamada de Funções **/
  criar_arquivo();
  menu();
  fechar_arquivo();
  
  return 0;
}

void criar_arquivo(){
  
  ::ArquivoConcessionaria = fopen("ArquivoConcessionaria.dat", "w+b");

  int tamanho = sizeof(struct Carro);
  FILE *arquivo_principal = fopen("dados.dat", "r+b");
  if(arquivo_principal != NULL){
    struct Pessoa * entidade = (struct Pessoa *) malloc( tamanho );
  
    fseek(arquivo_principal, 0, SEEK_SET);

    do{
      fread(entidade, tamanho, 1, arquivo_principal);
      fwrite(entidade, tamanho, 1, ArquivoConcessionaria);
    }while(!feof(arquivo_principal));

    fclose(arquivo_principal);
  }
}

void fechar_arquivo(){
  FILE *arquivo_principal = fopen("dados.dat", "w+b");

  struct Carro * entidade = (struct Carro *) malloc( sizeof(struct Carro));

  fseek(ArquivoConcessionaria, 0, SEEK_SET);
  do{
    fread(entidade, sizeof(struct Carro), 1, ArquivoConcessionaria);
    fwrite(entidade, sizeof(struct Carro), 1, arquivo_principal);
  }while(!feof(ArquivoConcessionaria));

  fclose(arquivo_principal);
  fclose(ArquivoConcessionaria);
}

void menu(){
  char tecla;
  while(tecla != 'Q' && tecla != 'q'){
	cout << "\n[X] Olá, bem vindo ao Crud de Concessionaria!" << endl;
    cout << "[1]Cadastrar." << endl;
    cout << "[2]Apresentar." << endl;
    cout << "[3]Alterar." << endl;
    cout << "[4]Excluir." << endl;
    cout << "[5]Sair." << endl;
    cout << "[X]Escolha uma opção! ";
    cin >> tecla;
    if(tecla == '1' || tecla == '2' || tecla == '3' || tecla == '4'){
      executar(tecla);
    };
    
    if (tecla == '5'){
		exit(0);
	}
	
	else{
		cout << "\n\n[X]Digite uma opção valida!" << endl;
	}
	
  }
}

void executar(char tecla){
  if(tecla == '1'){
    inserir();
  }else if(tecla == '2'){
    pesquisar();
  }
}

void inserir(){
  struct Carro * entidade = (struct Carro *) malloc( sizeof(struct Carro));
  
  cout << "\n[X]Opção de Cadastrar!" << endl;
  cout << "[X]Digite o codigo do carro: " << endl;
  cin >> entidade->Codigo;
  
  cout << "Digite o nome do carro: " << endl;
  cin >> entidade->Nome;
  
  cout << "Digite o ano do carro: " << endl;
  cin >> entidade->Ano;
  
  cout << "Digite o tipo de combustivel do carro: " << endl;
  cin >> entidade->Combustivel;
  
  cout << "Digite a configuração carro: " << endl;
  cin >> entidade->Configuracao;
  
  cout << "Digite a quantidade de lugares do carro: " << endl;
  cin >> entidade->Lugares;
  
  cout << "Digite o tipo de tração do carro: " << endl;
  cin >> entidade->Tracao;
  
  fseek(ArquivoConcessionaria, 0, SEEK_END);
  fwrite(entidade, sizeof(struct Carro), 1, ArquivoConcessionaria);
}

/**
SEEK_SET - Parte do início do arquivo e avança <n> bytes
SEEK_END - Parte do final do arquivo e retrocede <n> bytes
SEEK_CUR - Parte do local atual e avança <n> bytes
*/
void pesquisar(){
  struct Carro * entidade = (struct Carro *) malloc( sizeof(struct Carro));
  int codigo;
  cout << "\n[X]Opção de Apresentar!" << endl;
  cout << "[X]Digite o codigo: " << endl;
  
  cin >> codigo;
  fseek(ArquivoConcessionaria, 0, SEEK_SET);
  
  do{
    fread(entidade, sizeof(struct Carro), 1, ArquivoConcessionaria);
    if(codigo == entidade->Codigo){
      break;
    }
  }while(!feof(ArquivoConcessionaria));

  if(entidade != NULL && codigo == entidade->Codigo ){
    cout << "{" << endl;
    cout << "Codigo: "<< entidade->Codigo << "," << endl;
    cout << "Nome: "<< entidade->Nome << "," << endl;
    cout << "Ano: "<< entidade->Ano << "," << endl;
    cout << "Combustivel: "<< entidade->Combustivel << "," << endl;
    cout << "Configuração: "<< entidade->Configuracao << "," << endl;
    cout << "Lugares: "<< entidade->Lugares << "," << endl;
    cout << "Tração: "<< entidade->Tracao << "," << endl;
    cout << "}" << endl;
  }
  
  else{
    cout << "[X]Nenhum usuário registrado! ";
  }
  
  cout << endl;
}


```
