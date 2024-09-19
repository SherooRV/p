import javax.swing.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class Calculadora {

    // Interfaz para las operaciones
    interface Operacion {
        double calcular(double a, double b);
    }

    // Clases de operaciones
    static class Suma implements Operacion {
        public double calcular(double a, double b) {
            return a + b;
        }
    }

    static class Resta implements Operacion {
        public double calcular(double a, double b) {
            return a - b;
        }
    }

    static class Multiplicacion implements Operacion {
        public double calcular(double a, double b) {
            return a * b;
        }
    }

    static class Division implements Operacion {
        public double calcular(double a, double b) {
            if (b == 0) {
                throw new IllegalArgumentException("No se puede dividir entre cero");
            }
            return a / b;
        }
    }

    // Clase Contexto
    static class CalculadoraContexto {
        private Operacion operacion;

        public void setOperacion(Operacion operacion) {
            this.operacion = operacion;
        }

        public double calcular(double a, double b) {
            return operacion.calcular(a, b);
        }
    }

    public static void main(String[] args) {
        JFrame frame = new JFrame("Calculadora");
        frame.setSize(300, 300);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setLayout(null);

        JLabel labelA = new JLabel("Número A:");
        labelA.setBounds(20, 20, 100, 30);
        frame.add(labelA);

        JTextField textA = new JTextField();
        textA.setBounds(120, 20, 150, 30);
        frame.add(textA);

        JLabel labelB = new JLabel("Número B:");
        labelB.setBounds(20, 60, 100, 30);
        frame.add(labelB);

        JTextField textB = new JTextField();
        textB.setBounds(120, 60, 150, 30);
        frame.add(textB);

        JLabel labelResultado = new JLabel("Resultado:");
        labelResultado.setBounds(20, 100, 100, 30);
        frame.add(labelResultado);

        JTextField textResultado = new JTextField();
        textResultado.setBounds(120, 100, 150, 30);
        textResultado.setEditable(false);
        frame.add(textResultado);

        String[] operaciones = {"Suma", "Resta", "Multiplicación", "División"};
        JComboBox<String> comboOperaciones = new JComboBox<>(operaciones);
        comboOperaciones.setBounds(20, 140, 250, 30);
        frame.add(comboOperaciones);

        JButton buttonCalcular = new JButton("Calcular");
        buttonCalcular.setBounds(20, 180, 250, 30);
        frame.add(buttonCalcular);

        CalculadoraContexto calculadora = new CalculadoraContexto();

        buttonCalcular.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                double a = Double.parseDouble(textA.getText());
                double b = Double.parseDouble(textB.getText());
                String operacionSeleccionada = (String) comboOperaciones.getSelectedItem();

                switch (operacionSeleccionada) {
                    case "Suma":
                        calculadora.setOperacion(new Suma());
                        break;
                    case "Resta":
                        calculadora.setOperacion(new Resta());
                        break;
                    case "Multiplicación":
                        calculadora.setOperacion(new Multiplicacion());
                        break;
                    case "División":
                        calculadora.setOperacion(new Division());
                        break;
                }

                double resultado = calculadora.calcular(a, b);
                textResultado.setText(String.valueOf(resultado));
            }
        });

        frame.setVisible(true);
    }
}
