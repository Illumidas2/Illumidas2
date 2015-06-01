package org.rpm.generadores;
import java.util.ArrayList;
import java.util.List;
import java.util.Random;

/**
 *         Esta clase cumple con la normativa actual española.
 * 
 *         Decreto 2423/1975, de 25 de septiembre. Real Decreto 338/1990, de 9
 *         de marzo. Real Decreto 1624/1992, de 29 de diciembre que modifica el
 *         338/1990. Real Decreto 155/1996, de 2 de febrero. Orden de 3 de julio
 *         de 1998, por la que se modifica el Anexo del Decreto 2423/1975. Real
 *         Decreto 1065/2007, de 27 de julio. Orden EHA/451/2008, de 20 de
 *         febrero de 2008. Orden INT/2058/2008, de 14 de julio de 2008.
 */
public class GeneradorNIFE {
        private List<String> retVal;
        private Random semilla;
        public GeneradorNIFE() {
                semilla = new Random(System.currentTimeMillis());
        }
        /**
         * Obtiene una lista de strings con NIFs aleatorios
         * 
         * @param numDocuments
         * @return
         */
        public List<String> getNIFs(int numDocuments) {
                retVal = new ArrayList<String>();
                for (int i = 0; i < numDocuments; i++)
                        retVal.add(getNif());
                return retVal;
        }
        /**
         * Obtiene un número de DNI
         * 
         * @return Un número de DNI consta de 7 dígitos + 1 carácter de control
         */
        private String getNif() {
                Integer numDNI = new Integer(semilla.nextInt(100000000));
                return padLeft(numDNI.toString(), 8, '0') + letraDNI(numDNI);
        }
        /**
         * Cálculo de la letra del DNI/NIF
         * 
         * @param dni
         * @return
         */
        private String letraDNI(int dni) {
                int indice = dni % 23;
                return (new Character("TRWAGMYFPDXBNJZSQVHLCKE".charAt(indice)))
                                .toString();
        }
        /**
         * Obtiene una lista de strings con NIFs aleatorios
         * 
         * @param numDocuments
         * @return
         */
        public List<String> getNIEs(int numDocuments) {
                retVal = new ArrayList<String>();
                for (int i = 0; i < numDocuments; i++)
                        retVal.add(getNie());
                return retVal;
        }
        /**
         * Obtiene una lista de strings de CIF aleatorios
         * 
         * @param numCIFs
         * @return
         */
        public List<String> getCIFs(int numCIFs) {
                retVal = new ArrayList<String>();
                for (int i = 0; i < numCIFs; i++) {
                        retVal.add(getCIF());
                }
                return retVal;
        }
        /**
         * Crea un CIF Valido
         * 
         * @return
         */
        private String getCIF() {
                Integer numDNI = new Integer(semilla.nextInt(10000000));
                String padLeft = padLeft(numDNI.toString(), 7, '0');
                char letraIzq = letraCIF();
                return (new Character(letraIzq)).toString() + padLeft
                                + numeroCIF(padLeft, letraIzq);
        }
        /**
         * Obtiene el digito de control de un CIF
         * 
         * @param numero
         *            sin digito de control
         * @param letra
         * @return
         */
        private String numeroCIF(String numero, char letra) {
                int sumaPares = 0;
                int sumaImpares = 0;
                int sumaTotal = 0;
                int ultimo = 0;
                for (int i = 0; i < numero.length(); i++) {
                        if ((i + 1) % 2 == 0) {
                                // si la posicion es par
                                sumaPares += Integer.parseInt((new Character(numero.charAt(i)))
                                                .toString());
                        } else {
                                sumaImpares += sumarDigitos(Integer.parseInt((new Character(
                                                numero.charAt(i))).toString()) * 2);
                        }
                }
                sumaTotal = sumaImpares + sumaPares;
                String st = (new Integer(sumaTotal)).toString();
                ultimo = 10 - Integer.parseInt((new Character(st
                                .charAt(st.length() - 1))).toString());
                if (ultimo == 10)
                        ultimo = 0;
                String solucion;
                String letras = "JABCDEFGHI";
                switch (letra) {
                case 'K':
                case 'P':
                case 'Q':
                case 'S':
                        // letra
                        solucion = (new Character(letras.charAt(ultimo))).toString();
                        break;
                case 'A':
                case 'B':
                case 'E':
                case 'H':
                        // numero
                        solucion = (new Integer(ultimo)).toString();
                        break;
                default:
                        // numero o letra
                        if (semilla.nextInt(2) % 2 == 0) {
                                solucion = (new Character(letras.charAt(ultimo))).toString();
                        } else {
                                solucion = (new Integer(ultimo)).toString();
                        }
                        break;
                }
                return solucion;
        }
        private int sumarDigitos(int i) {
                int suma = i;
                Integer numero = new Integer(i);
                String n = numero.toString();
                if (n.length() == 2) {
                        suma = (new Integer((new Character(n.charAt(0))).toString()))
                                        + (new Integer((new Character(n.charAt(1))).toString()));
                }
                return suma;
        }
        /**
         * Genera de forma aleatoria un numero CIF
         * 
         * @return
         */
        private char letraCIF() {
                String letras = "ABCDEFGHJKLMNPRQSUVW";
                int i = semilla.nextInt(letras.length());
                return letras.charAt(i);
        }
        /**
         * Esta funcion genera un NIE
         * 
         * @return NIE
         */
        private String getNie() {
                Integer numDNI = new Integer(semilla.nextInt(10000000));
                Integer codigo = new Integer(semilla.nextInt(3));
                Integer numero = Integer.parseInt(codigo.toString()
                                + padLeft(numDNI.toString(), 7, '0'));
                return (new Character(letraNie(codigo))).toString()
                                + padLeft(numDNI.toString(), 7, '0') + letraDNI(numero);
        }
        /**
         * Obtiene una letra aleatoria para el NIE
         * 
         * @return
         */
        private char letraNie(int codigo) {
                String nie = "XYZ";
                // se incluye la Z por la normativa EHA/451/2008
                return nie.charAt(codigo);
        }
        public List<String> getDocuments(int numNifs, int numNies, int numCIFs) {
                // Es curioso. Primero he intentado:
                // retVal = GetNIFs(numNifs);
                // retVal.AddRange(GetNIEs(numNies));
                // Pero sólo devuelve los Nies
                List<String> nies = getNIEs(numNies);
                retVal = getNIFs(numNifs);
                retVal.addAll(nies);
                List<String> cif = getCIFs(numCIFs);
                retVal.addAll(cif);
                return retVal;
        }
        public static String padRight(String str, int size, char padChar) {
                StringBuffer padded = new StringBuffer(str);
                while (padded.length() < size) {
                        padded.append(padChar);
                }
                return padded.toString();
        }
        public static String padLeft(String str, int size, char padChar) {
                StringBuffer padded = new StringBuffer();
                int newSize = size - str.length();
                while (padded.length() < newSize) {
                        padded.append(padChar);
                }
                padded.append(str);
                return padded.toString();
        }
}