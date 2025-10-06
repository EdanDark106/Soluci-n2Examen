import java.util.HashMap;
import java.util.Map;

class Parqueo {
    private int nv; // Número de vehículos
    private double tarifaHora; // Tarifa por hora
    private Map<String, String> vehiculoConductor; // Mapa para placa y conductor
    private Map<String, Double> vehiculoTiempo; // Mapa para placa y tiempo (en horas)

    public Parqueo(int nv, double tarifaHora) {
        this.nv = nv;
        this.tarifaHora = tarifaHora;
        this.vehiculoConductor = new HashMap<>();
        this.vehiculoTiempo = new HashMap<>();
    }

    // c) Método para registrar el ingreso de un vehículo con placa P y conductor
    public boolean ingreso(String placa, String conductor) {
        if (nv < 5 && !vehiculoConductor.containsKey(placa)) { // Límite de 5 vehículos
            vehiculoConductor.put(placa, conductor);
            vehiculoTiempo.put(placa, 0.0); // Tiempo inicial
            nv++;
            return true;
        }
        return false;
    }

    // d) Método sobrecargado para registrar ingreso especificando las horas
    public boolean ingreso(String placa, String conductor, double horas) {
        if (nv < 5 && !vehiculoConductor.containsKey(placa) && horas >= 0) {
            vehiculoConductor.put(placa, conductor);
            vehiculoTiempo.put(placa, horas);
            nv++;
            return true;
        }
        return false;
    }

    // d) Hallar al conductor del vehículo de placa Z
    public String hallarConductor(String placa) {
        return vehiculoConductor.getOrDefault(placa, "No encontrado");
    }

    // d) Hallar cuántos vehículos permanecieron M horas en el parqueo
    public int contarVehiculosPorHoras(double m) {
        int count = 0;
        for (double tiempo : vehiculoTiempo.values()) {
            if (tiempo == m) count++;
        }
        return count;
    }

    // Mostrar estado del parqueo
    public void mostrarEstado() {
        System.out.println("Vehículos en parqueo: " + nv);
        for (Map.Entry<String, String> entry : vehiculoConductor.entrySet()) {
            String placa = entry.getKey();
            System.out.println("Placa: " + placa + ", Conductor: " + entry.getValue() +
                             ", Tiempo: " + vehiculoTiempo.get(placa) + " horas");
        }
    }
}

public class Main {
    public static void main(String[] args) {
        // Instanciar un objeto Parqueo con nv=5 y tarifaHora=12.5
        Parqueo parqueo = new Parqueo(5, 12.5);

        // Agregar vehículos del ejemplo
        parqueo.ingreso("1245AXC", "Luis Jairo", 2.0);
        parqueo.ingreso("3456BVC", "Marcia Lira", 9.0);
        parqueo.ingreso("257eJUX", "Pablo Rubio", 10.0);
        parqueo.ingreso("322fHIP", "Rosa Jux", 10.0);
        parqueo.ingreso("3465KIX", "Saul Lopez", 11.0);

        // Mostrar estado inicial
        System.out.println("Estado inicial del parqueo:");
        parqueo.mostrarEstado();

        // Prueba de hallar conductor
        String conductor = parqueo.hallarConductor("1245AXC");
        System.out.println("\nConductor de placa 1245AXC: " + conductor);

        // Prueba de contar vehículos que permanecieron 10 horas
        int count10Horas = parqueo.contarVehiculosPorHoras(10.0);
        System.out.println("Vehículos que permanecieron 10 horas: " + count10Horas);

        // Prueba de ingreso con nueva placa y conductor
        boolean nuevoIngreso = parqueo.ingreso("789XYZ", "Nuevo Conductor", 5.0);
        System.out.println("\nNuevo ingreso exitoso: " + nuevoIngreso);

        // Mostrar estado final
        System.out.println("\nEstado final del parqueo:");
        parqueo.mostrarEstado();
    }
}
