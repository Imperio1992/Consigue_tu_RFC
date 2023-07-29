# Consigue_tu_RFC

#include <iostream>
#include <string>
using namespace std;

class Empleado {
private:
    string nombre;
    string apellidoPaterno;
    string apellidoMaterno;
    string fechaNacimiento;

public:
    Empleado(string nombre, string apellidoPaterno, string apellidoMaterno, string fechaNacimiento) {
        this->nombre = nombre;
        this->apellidoPaterno = apellidoPaterno;
        this->apellidoMaterno = apellidoMaterno;
        this->fechaNacimiento = fechaNacimiento;
    }

    // Método para calcular y retornar el RFC
    string calcularRFC() {
        string rfc;

        // Primeros 2 caracteres (VE)
        rfc.push_back(apellidoPaterno[0]);
        for (char c : apellidoPaterno) {
            if (c == 'A' || c == 'E' || c == 'I' || c == 'O' || c == 'U') {
                rfc.push_back(c);
                break;
            }
        }

        // 3ra posición: Inicial del apellido materno o X si no hay apellido materno
        if (!apellidoMaterno.empty()) {
            rfc.push_back(apellidoMaterno[0]);
        } else {
            rfc.push_back('X');
        }

        // 4a posición: Inicial del primer nombre o X si no hay primer nombre
        if (!nombre.empty()) {
            rfc.push_back(nombre[0]);
        } else {
            rfc.push_back('X');
        }

        // Validar que fechaNacimiento tenga al menos 10 caracteres (dd/mm/aaaa)
        if (fechaNacimiento.length() >= 10) {
            // 5a y 6a posición: Dos últimos dígitos del año de nacimiento
            rfc += fechaNacimiento.substr(8, 2);

            // 7a y 8a posición: Mes de nacimiento
            rfc += fechaNacimiento.substr(3, 2);

            // 9a y 10a posición: Día de nacimiento
            rfc += fechaNacimiento.substr(0, 2);
        } else {
            cout << "Error: La fecha de nacimiento debe tener el formato dd/mm/aaaa." << endl;
            rfc = "INVALIDO";
        }

        return rfc;
    }
};

int main() {
    string nombre, apellidoPaterno, apellidoMaterno, fechaNacimiento;
    cout << "Calculo de RFC" << endl;
    cout << "Introduce tu Nombre: ";
    getline(cin, nombre);
    cout << "Introduce tu Apellido Paterno: ";
    getline(cin, apellidoPaterno);
    cout << "Introduce tu Apellido Materno: ";
    getline(cin, apellidoMaterno);
    cout << "Fecha de Nacimiento (Solo Numeros en formato dd/mm/aaaa): ";
    getline(cin, fechaNacimiento);

    Empleado empleado(nombre, apellidoPaterno, apellidoMaterno, fechaNacimiento);
    string rfc = empleado.calcularRFC();

    if (rfc != "INVALIDO") {
        cout << "Tu RFC sin Homoclave es: " << rfc << endl;
    }

    return 0;
}
