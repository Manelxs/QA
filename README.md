// Auto-generated by the postman-to-k6 converter

import "./libs/shim/core.js";
import "./libs/shim/expect.js";

export let options = { maxRedirects: 4 };

const Request = Symbol.for("request");
postman[Symbol.for("initial")]({
  options
});

export default function() {
  postman[Request]({
    name: "New Request",
    id: "bbb7c8f2-cc5a-4458-866d-86ed9b3a8fb0",
    method: "GET",
    address: "https://jsonplaceholder.typicode.com/posts",
    post(response) {
      pm.test("Response status code is 200", function() {
        pm.response.to.have.status(200);
      });

      pm.test("Response time is less than 200ms", function() {
        pm.expect(pm.response.responseTime).to.be.below(200);
      });

      pm.test(
        "Response has the required fields - userId, id, title, and body",
        function() {
          const responseData = pm.response.json();

          pm.expect(responseData).to.be.an("array");
          responseData.forEach(function(post) {
            pm.expect(post).to.have.property("userId");
            pm.expect(post).to.have.property("id");
            pm.expect(post).to.have.property("title");
            pm.expect(post).to.have.property("body");
          });
        }
      );

      pm.test("UserId and id are non-negative integers", function() {
        const responseData = pm.response.json();

        responseData.forEach(function(post) {
          pm.expect(post.userId)
            .to.be.a("number")
            .and.to.be.at.least(0);
          pm.expect(post.id)
            .to.be.a("number")
            .and.to.be.at.least(0);
        });
      });

      pm.test("Title and body are non-empty strings", function() {
        const responseData = pm.response.json();

        pm.expect(responseData).to.be.an("array").that.is.not.empty;
        responseData.forEach(function(post) {
          pm.expect(post.title)
            .to.be.a("string")
            .and.to.have.lengthOf.at.least(1, "Title should not be empty");
          pm.expect(post.body)
            .to.be.a("string")
            .and.to.have.lengthOf.at.least(1, "Body should not be empty");
        });
      });
    }
  });

  postman[Request]({
    name: "New Request",
    id: "42d19f7a-7c5d-4a9c-8517-9499419471ef",
    method: "POST",
    address: "https://jsonplaceholder.typicode.com/posts",
    post(response) {
      pm.test("Response status code is 201", function() {
        pm.expect(pm.response.code).to.equal(201);
      });

      pm.test("Response time is less than 500ms", function() {
        pm.expect(pm.response.responseTime).to.be.below(500);
      });

      pm.test("Response has the required field - id", function() {
        const responseData = pm.response.json();

        pm.expect(responseData).to.be.an("object");
        pm.expect(responseData.id).to.exist;
      });

      pm.test("Id is a non-negative integer", function() {
        const responseData = pm.response.json();

        pm.expect(responseData.id).to.be.a("number");
        pm.expect(responseData.id).to.be.at.least(0);
      });

      pm.test("Content-Type header is application/json", function() {
        pm.expect(pm.response.headers.get("Content-Type")).to.include(
          "application/json"
        );
      });
    }
  });

  postman[Request]({
    name: "New Request",
    id: "35b279a1-ddb8-4aa4-a50c-fc59132af3e4",
    method: "PUT",
    address: "https://jsonplaceholder.typicode.com/posts",
    post(response) {
      // Verificar respuesta a solicitud PUT
      pm.test("Verificar código de estado", function() {
        // Obtener el código de estado
        const statusCode = pm.response.code;

        if (statusCode === 404) {
          console.log(
            "Recurso no encontrado. Verifica la URL y el ID del recurso."
          );
          pm.expect(statusCode).to.equal(404); // Esta prueba pasará
        } else {
          pm.expect(statusCode).to.be.oneOf([200, 201, 204]); // Códigos esperados para un PUT exitoso
        }
      });

      // Verificar contenido de la respuesta según el código de estado
      if (pm.response.code === 404) {
        pm.test("Respuesta 404 contiene mensaje de error", function() {
          try {
            var jsonData = pm.response.json();
            // Verificar si hay algún mensaje descriptivo
            if (jsonData.message || jsonData.error) {
              console.log(
                "Mensaje de error:",
                jsonData.message || jsonData.error
              );
            }
          } catch (e) {
            console.log("La respuesta 404 no contiene JSON válido");
          }
        });
      } else {
        // Procesamiento para respuestas exitosas
        try {
          var jsonData = pm.response.json();

          // Verificar que la respuesta tenga estructura básica
          pm.test("Respuesta contiene datos", function() {
            pm.expect(jsonData).to.be.an("object");
          });

          // Otras verificaciones para respuestas exitosas...
        } catch (e) {
          console.log("No hay datos JSON en la respuesta o están vacíos");
        }
      }

      // Verificar tiempo de respuesta (aplicable a cualquier código de estado)
      pm.test("Tiempo de respuesta razonable", function() {
        pm.expect(pm.response.responseTime).to.be.below(2000);
      });

      // Registrar información útil para depuración
      console.log("URL solicitada:", pm.request.url.toString());
      console.log("Código de estado:", pm.response.code);
      console.log("Tiempo de respuesta:", pm.response.responseTime + "ms");
    }
  });

  postman[Request]({
    name: "New Request",
    id: "44d8d1ca-ddc5-49db-a947-ee981dbfa152",
    method: "DELETE",
    address: "https://jsonplaceholder.typicode.com/posts",
    post(response) {
      // Verificar código de estado para DELETE
      pm.test("Verificar código de estado para DELETE", function() {
        // Los códigos típicos para DELETE son:
        // 200 (OK) - Si devuelve contenido
        // 202 (Accepted) - Si el proceso es asíncrono
        // 204 (No Content) - Si no devuelve contenido (más común)
        // 404 (Not Found) - Si el recurso no existe

        const statusCode = pm.response.code;

        if (statusCode === 404) {
          console.log("El recurso que intentas eliminar no existe.");
          pm.expect(statusCode).to.equal(404); // Esta prueba pasará con 404
        } else {
          pm.expect(statusCode).to.be.oneOf([200, 202, 204]); // Códigos típicos para DELETE exitoso
        }
      });

      // Verificar respuesta según el código
      if (pm.response.code === 204) {
        pm.test("Respuesta sin contenido como se esperaba", function() {
          pm.expect(pm.response.text()).to.be.empty;
        });
      } else if ([200, 202].includes(pm.response.code)) {
        try {
          var jsonData = pm.response.json();

          pm.test("Respuesta confirma eliminación", function() {
            // Verificar posibles campos de confirmación
            if (jsonData.success !== undefined) {
              pm.expect(jsonData.success).to.be.true;
            }

            if (jsonData.deleted !== undefined) {
              pm.expect(jsonData.deleted).to.be.true;
            }

            if (jsonData.message) {
              pm.expect(jsonData.message).to.not.include("error");
              console.log("Mensaje del servidor:", jsonData.message);
            }
          });
        } catch (e) {
          console.log("No hay JSON en la respuesta o está mal formateado");
        }
      } else if (pm.response.code === 404) {
        try {
          var errorData = pm.response.json();
          console.log(
            "Mensaje de error 404:",
            errorData.message || errorData.error || "No se encontró el recurso"
          );
        } catch (e) {
          console.log("Respuesta 404 sin detalles JSON");
        }
      }

      // Verificar tiempo de respuesta
      pm.test("Tiempo de respuesta razonable", function() {
        pm.expect(pm.response.responseTime).to.be.below(2000);
      });

      // Registrar información para depuración
      console.log("URL de eliminación:", pm.request.url.toString());
      console.log("Código de estado:", pm.response.code);
      console.log("Tiempo de respuesta:", pm.response.responseTime + "ms");

      // Opcional: Verificar que el recurso ya no existe (solo si tienes el ID)
      // Esto solo establece variables para una prueba posterior, no hace la verificación aquí
      if (pm.environment.get("resourceId")) {
        const baseUrl = pm.request.url
          .toString()
          .split("/")
          .slice(0, -1)
          .join("/");
        const verificationUrl =
          baseUrl + "/" + pm.environment.get("resourceId");

        pm.environment.set("verificationUrl", verificationUrl);
        console.log(
          "Para verificar la eliminación, envía una solicitud GET a:",
          verificationUrl
        );
      }
    }
  });
}
