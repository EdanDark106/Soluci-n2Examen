import java.util.HashMap;
import java.util.Map;

class Parqueo {
    private int nv; // Número de vehículos
    private double tarifaHora; // Tarifa por hora
    private Map<String, Double> vehiculo; // Mapa para almacenar placa y tiempo (en horas)

    public Parqueo(int nv, double tarifaHora) {
        this.nv = nv;
        this.tarifaHora = tarifaHora;
        this.vehiculo = new HashMap<>();
    }

    // Método para registrar la salida de un vehículo por placa
    public double salida(String placa) {
        if (vehiculo.containsKey(placa)) {
            double tiempo = vehiculo.get(placa); // Tiempo en horas
            double costo = tiempo * tarifaHora;
            vehiculo.remove(placa); // Elimina el vehículo al salir
            nv--; // Reduce el número de vehículos
            return costo;
        }
        return 0.0; // Si no existe la placa
    }

    // Método sobrecargado para registrar salida y cancelar (simulado)
    public boolean salida(String placa, boolean cancelar) {
        if (cancelar && vehiculo.containsKey(placa)) {
            vehiculo.remove(placa); // Cancela el registro
            nv--; // Reduce el número de vehículos
            return true;
        }
        return false; // Si no se puede cancelar
    }

    // Método para agregar un vehículo con su tiempo
    public void agregarVehiculo(String placa, double tiempo) {
        if (tiempo >= 0 && nv < 5) { // Límite de 5 vehículos como ejemplo
            vehiculo.put(placa, tiempo);
            nv++;
        }
    }

    // Calcular ingreso total de todos los vehículos
    public double calcularIngresoTotal() {
        double total = 0.0;
        for (double tiempo : vehiculo.values()) {
            total += tiempo * tarifaHora;
        }
        return total;
    }

    // Calcular ingreso total para vehículos que estuvieron k horas
    public double calcularIngresoPorHoras(double k) {
        double total = 0.0;
        for (Map.Entry<String, Double> entry : vehiculo.entrySet()) {
            if (entry.getValue() == k) {
                total += k * tarifaHora;
            }
        }
        return total;
    }

    // Mostrar estado del parqueo
    public void mostrarEstado() {
        System.out.println("Vehículos en parqueo: " + nv);
        for (Map.Entry<String, Double> entry : vehiculo.entrySet()) {
            System.out.println("Placa: " + entry.getKey() + ", Tiempo: " + entry.getValue() + " horas");
        }
    }
}

public class Main {
    public static void main(String[] args) {
        // Instanciar un objeto Parqueo con nv=5 y tarifaHora=12.5
        Parqueo parqueo = new Parqueo(5, 12.5);

        // Agregar vehículos del ejemplo
        parqueo.agregarVehiculo("1245AXC", 2.0);
        parqueo.agregarVehiculo("3456BVC", 9.0);
        parqueo.agregarVehiculo("257eJUX", 10.0);
        parqueo.agregarVehiculo("322fHIP", 10.0);
        parqueo.agregarVehiculo("3465KIX", 10.0);

        // Mostrar estado inicial
        System.out.println("Estado inicial del parqueo:");
        parqueo.mostrarEstado();

        // a) Sobrecargar un operador para implementar la salida de un vehículo con placa "1245AXC"
        double costoSalida = parqueo.salida("1245AXC");
        System.out.println("\nCosto de salida del vehículo 1245AXC: " + costoSalida + " Bs");

        // b) Sobrecargar un método para cancelar la salida de un vehículo con placa "3456BVC"
        boolean cancelado = parqueo.salida("3456BVC", true);
        System.out.println("Cancelación de vehículo 3456BVC: " + (cancelado ? "Éxito" : "Fallo"));

        // Calcular ingresos
        System.out.println("\nIngreso total de todos los vehículos: " + parqueo.calcularIngresoTotal() + " Bs");
        System.out.println("Ingreso total para vehículos que estuvieron 10 horas: " + parqueo.calcularIngresoPorHoras(10.0) + " Bs");

        // Mostrar estado final
        System.out.println("\nEstado final del parqueo:");
        parqueo.mostrarEstado();
    }
}
