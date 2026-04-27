# BeautyManager
Sistema de agendamento para funcionários da área da beleza
#include <iostream>
#include <fstream>
#include <string>
using namespace std;

struct Cliente {
    string nome;
    string servico;
    string horario;
    string data;
};

Cliente agenda[100];
int total = 0;

// ---------------- SALVAR ----------------
void salvarArquivo() {
    ofstream arquivo("clientes.txt");

    for (int i = 0; i < total; i++) {
        arquivo << agenda[i].nome << endl;
        arquivo << agenda[i].servico << endl;
        arquivo << agenda[i].horario << endl;
        arquivo << agenda[i].data << endl;
    }

    arquivo.close();
}

// ---------------- CARREGAR ----------------
void carregarArquivo() {
    ifstream arquivo("clientes.txt");

    while (getline(arquivo, agenda[total].nome)) {
        getline(arquivo, agenda[total].servico);
        getline(arquivo, agenda[total].horario);
        getline(arquivo, agenda[total].data);
        total++;
    }

    arquivo.close();
}

// ---------------- CADASTRAR ----------------
void cadastrarCliente() {
    string nome, servico, horario, data;
    bool ocupado = false;

    cout << "\nNome: ";
    getline(cin, nome);

    cout << "Servico: ";
    getline(cin, servico);

    cout << "Horario: ";
    getline(cin, horario);

    cout << "Data: ";
    getline(cin, data);

    for (int i = 0; i < total; i++) {
        if (agenda[i].horario == horario && agenda[i].data == data) {
            ocupado = true;
        }
    }

    if (ocupado) {
        cout << "\nHorario indisponivel!\n";
    } else {
        agenda[total].nome = nome;
        agenda[total].servico = servico;
        agenda[total].horario = horario;
        agenda[total].data = data;
        total++;

        salvarArquivo();

        cout << "\nAgendamento realizado com sucesso!\n";
    }
}

// ---------------- LISTAR ----------------
void listarAgenda() {
    cout << "\n------ AGENDA ------\n";

    if (total == 0) {
        cout << "Nenhum agendamento.\n";
        return;
    }

    for (int i = 0; i < total; i++) {
        cout << i + 1 << ". "
             << agenda[i].nome << " | "
             << agenda[i].servico << " | "
             << agenda[i].horario << " | "
             << agenda[i].data << endl;
    }
}

// ---------------- BUSCAR ----------------
void buscarCliente() {
    string nome;
    bool encontrou = false;

    cout << "\nDigite o nome: ";
    getline(cin, nome);

    for (int i = 0; i < total; i++) {
        if (agenda[i].nome == nome) {
            cout << agenda[i].nome << " | "
                 << agenda[i].servico << " | "
                 << agenda[i].horario << " | "
                 << agenda[i].data << endl;

            encontrou = true;
        }
    }

    if (!encontrou) {
        cout << "Cliente nao encontrado.\n";
    }
}

// ---------------- CANCELAR ----------------
void cancelarAgendamento() {
    int num;

    cout << "\nNumero para cancelar: ";
    cin >> num;
    cin.ignore();

    if (num < 1 || num > total) {
        cout << "Numero invalido!\n";
        return;
    }

    for (int i = num - 1; i < total - 1; i++) {
        agenda[i] = agenda[i + 1];
    }

    total--;
    salvarArquivo();

    cout << "Agendamento cancelado!\n";
}

// ---------------- EDITAR ----------------
void editarAgendamento() {
    int num;

    cout << "\nNumero para editar: ";
    cin >> num;
    cin.ignore();

    if (num < 1 || num > total) {
        cout << "Numero invalido!\n";
        return;
    }

    cout << "Novo nome: ";
    getline(cin, agenda[num - 1].nome);

    cout << "Novo servico: ";
    getline(cin, agenda[num - 1].servico);

    cout << "Novo horario: ";
    getline(cin, agenda[num - 1].horario);

    cout << "Nova data: ";
    getline(cin, agenda[num - 1].data);

    salvarArquivo();

    cout << "Agendamento atualizado!\n";
}

// ---------------- HORARIOS LIVRES ----------------
void horariosLivres() {
    string dataBusca;
    bool ocupado;

    string horarios[] = {
        "09:00", "10:00", "11:00",
        "13:00", "14:00", "15:00",
        "16:00", "17:00"
    };

    cout << "\nDigite a data: ";
    getline(cin, dataBusca);

    cout << "\nHorarios livres em " << dataBusca << ":\n";

    for (int h = 0; h < 8; h++) {
        ocupado = false;

        for (int i = 0; i < total; i++) {
            if (agenda[i].data == dataBusca &&
                agenda[i].horario == horarios[h]) {
                ocupado = true;
            }
        }

        if (!ocupado) {
            cout << horarios[h] << endl;
        }
    }
}

// ---------------- MENU ----------------
int main() {
    carregarArquivo();

    int opcao;

    do {
        cout << "\n=========================\n";
        cout << "   SALAO DE BELEZA\n";
        cout << "=========================\n";
        cout << "1. Cadastrar cliente\n";
        cout << "2. Listar agenda\n";
        cout << "3. Buscar cliente\n";
        cout << "4. Cancelar agendamento\n";
        cout << "5. Editar agendamento\n";
        cout << "6. Horarios livres\n";
        cout << "0. Sair\n";
        cout << "Opcao: ";

        cin >> opcao;
        cin.ignore();

        switch(opcao) {
            case 1: cadastrarCliente(); break;
            case 2: listarAgenda(); break;
            case 3: buscarCliente(); break;
            case 4: cancelarAgendamento(); break;
            case 5: editarAgendamento(); break;
            case 6: horariosLivres(); break;
            case 0: cout << "\nEncerrando sistema...\n"; break;
            default: cout << "\nOpcao invalida!\n";
        }

    } while(opcao != 0);

    return 0;
}
