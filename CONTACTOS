export default {
  async fetch(request, env) {
    if (request.method === "POST") {
      const formData = await request.formData();
      const matricula = formData.get("matricula");
      
      const apiResponse = await fetch("https://script.google.com/macros/s/AKfycbyCP0zlB95ADZk1kOZ5qZFh7LSHfMUwi0S3bPFLXai2TXCQw_guuO3-PpvGf3Y_LMPPBQ/exec", {
        method: "POST", body: JSON.stringify({ action: "webhook", matricula: matricula })
      });
      const validacion = await apiResponse.json();

      if (validacion.status === "error") return new Response(`🛑 ACCESO DENEGADO`, { status: 403 });

      const registroFinal = {
        matricula: matricula, nombre_oficial: validacion.nombre,
        fecha: new Date().toLocaleString("es-MX", { timeZone: "America/Mexico_City" })
      };
      await env.CONTACTOS.put(matricula, JSON.stringify(registroFinal));
      
      return new Response(`✅ ¡Identidad Confirmada!\nBienvenido ${validacion.nombre}.`, { headers: { "Access-Control-Allow-Origin": "*" } });
    }
    return new Response("API Edge Activa.");
  }
};
