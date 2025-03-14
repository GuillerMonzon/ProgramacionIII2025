# ProgramacionIII2025
LABORATORIO 1 - NOTACIONES INFIJAS-PREFIJAS
*
 * Click nbfs://nbhost/SystemFileSystem/Templates/Licenses/license-default.txt to change this license
 */

package com.mycompany.notaciones;

/**
 *
 * @author GUILLERMO
 */
import java.util.Stack;

public class Notaciones {

    public static boolean isOperador(char c) {
        return c == '+' || c == '-' || c == '*' || c == '/';
    }
    
    public static String reverseString(String str) {
        return new StringBuilder(str).reverse().toString();
    }

    public static int validateHierarchy(char operador) {
        switch (operador) {
            case '+':
            case '-':
                return 1;
            case '*':
            case '/':
                return 2;
            default:
                return -1;
        }
    }
    
    public static String convertToPostfix(String expresionInfija) {
        StringBuilder resultado = new StringBuilder();
        Stack<Character> pila = new Stack<>();

        for (int i = 0; i < expresionInfija.length(); i++) {
            char c = expresionInfija.charAt(i);

            if (Character.isLetterOrDigit(c)) {
                resultado.append(c);
            }
            
            else if (c == '(') {
                pila.push(c);
            }
            
            else if (c == ')') {
                while (!pila.isEmpty() && pila.peek() != '(') {
                    resultado.append(pila.pop());
                }
                if (!pila.isEmpty() && pila.peek() != '(') {
                    return "Expresión inválida"; // Expresión inválida
                } else {
                    pila.pop();
                }
            }
            
            else if (isOperador(c)) {
                while (!pila.isEmpty() && validateHierarchy(c) <= validateHierarchy(pila.peek())) {
                    resultado.append(pila.pop());
                }
                pila.push(c);
            }
        }

        while (!pila.isEmpty()) {
            resultado.append(pila.pop());
        }

        return resultado.toString();
    }

    public static String convertToPrefix(String expresionInfija) {
       
        String expresionInvertida = reverseString(expresionInfija);

        expresionInvertida = expresionInvertida.replace('(', '#').replace(')', '(').replace('#', ')');

        String expresionPostfija = convertToPostfix(expresionInvertida);

        return reverseString(expresionPostfija);
    }

    public static void main(String[] args) {
        String expresionInfija = "A+B+C+D*F/E";

        System.out.println("Infija: " + expresionInfija);
        System.out.println("Postfija: " + convertToPostfix(expresionInfija));
        System.out.println("Prefija: " + convertToPrefix(expresionInfija));
    }
}
