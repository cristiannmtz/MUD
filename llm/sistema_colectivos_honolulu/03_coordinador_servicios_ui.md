# 03 - Coordinador de Aplicación, Servicios y UI

Este documento explica cómo `sistema_colectivos_honolulu` logra separar la lógica del sistema de la interfaz de usuario, permitiendo que la aplicación funcione independientemente del entorno gráfico.

---

## 🎯 1. El Rol de `CoordinadorApp` ("El Gerente")

`CoordinadorApp` es el cerebro del sistema. Realiza cuatro tareas esenciales:
1. **Ensambla dependencias:** Llama a `Factory.getInstancia(...)` para obtener los DAOs y se los inyecta a los servicios (`ParadaServiceImpl`, etc.).
2. **Carga inicial de datos:** Si es necesario, recupera los datos maestros y los mantiene en caché o mapas en memoria.
3. **Crea y enlaza la Vista:** Instancia la implementación de `Interfaz` (ej. `InterfazJavaFXImpl`) y se inyecta a sí mismo con `interfaz.setCoordinador(this)`.
4. **Expone métodos de negocio a la UI:** La UI no habla con los servicios directamente; invoca métodos de alto nivel en el Coordinador (ej. `coordinador.calcularRecorrido(...)`).

> **Regla Crítica:** En `CoordinadorApp.java` **no existe un solo `import javafx...` ni referencias a nodos visuales (Stage, Scene, Button)**. Es 100% código Java estándar.

---

## 💼 2. La Capa de Servicios (`*Service` y `*ServiceImpl`)

Los servicios aíslan las operaciones CRUD de los DAOs de la lógica de aplicación:
- **Interfaz `ParadaService`:** Define el contrato de casos de uso (`insertar`, `actualizar`, `borrar`, `buscarTodos`, `buscarPorCodigo`, etc.).
- **Implementación `ParadaServiceImpl`:**
  - Recibe el DAO en su constructor:
    ```java
    public ParadaServiceImpl(ParadaDAO paradaDAO) {
        this.paradaDAO = paradaDAO;
    }
    ```
  - Aplica validaciones de reglas de negocio antes de persistir (ej. verificar que el código no exista o no esté duplicado).

---

## 🖥️ 3. El Contrato de UI (`Interfaz.java`)

Para que el Coordinador no dependa de JavaFX, se define la siguiente interfaz:

```java
package colectivo.ui;

import colectivo.controlador.CoordinadorApp;

public interface Interfaz {
    void setCoordinador(CoordinadorApp coordinador);
    void iniciar();
}
```

### Cómo se lanza JavaFX sin acoplar el Main
`InterfazJavaFXImpl` implementa `Interfaz`. En su método `iniciar()`, delega en un lanzador o llama a `Application.launch(JavaFXLauncher.class)`:
- Esto resuelve el problema clásico de JavaFX donde la clase `Application` intenta forzar el ciclo de vida del programa.
- En este modelo, el ciclo de vida lo controla **`AplicacionPrincipal -> CoordinadorApp`**, y JavaFX es solo un esclavo visual que se levanta cuando el Coordinador lo decide.
- Si mañana se quisiera hacer un cliente de consola para jugar MUD por terminal (ej. telnet o stdin/stdout), basta con crear `InterfazConsolaImpl implements Interfaz` sin tocar una sola línea del Coordinador.

---

## 🪟 4. Presentadores y Componentes Visuales

- Cada vista compleja tiene su propio presentador (patrón MVP: Model-View-Presenter), como `PresentadorVistaPrincipal.java`.
- Los componentes reutilizables se encapsulan en el subpaquete `ui.componentes` (ej. `MapViewer.java`, `RecorridoCard.java`, `PantallaCarga.java`).
- Los textos visuales no se escriben fijos en el código (hardcoded): se recuperan mediante `MessageManager.getInstance().getMessage("clave")` para dar soporte multilenguaje (i18n).
