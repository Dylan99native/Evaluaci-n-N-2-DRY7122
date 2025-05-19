# Evaluaci-n-N-2-DRY7122
Javier Silva-Kevin Bruna-Carlos Beltran
import requests

key = "32de3f71-bd90-41a4-8c4e-79f3ef75c53e"

# Diccionario de ciudades conocidas con coordenadas
ciudades = {
    "santiago": "-33.4489,-70.6693",
    "ovalle": "-30.6016,-71.2005"
}

def obtener_datos_ruta(origen, destino):
    if origen not in ciudades or destino not in ciudades:
        print("❌ Ciudad no reconocida. Usa: santiago, ovalle")
        return

    coord_origen = ciudades[origen]
    coord_destino = ciudades[destino]

    url = f"https://graphhopper.com/api/1/route?point={coord_origen}&point={coord_destino}&vehicle=car&locale=es&key={key}"
    
    try:
        response = requests.get(url)
        data = response.json()

        if 'paths' not in data:
            print("❌ La API no devolvió resultados válidos.")
            print("Respuesta de la API:", data)
            return

        distancia_km = data['paths'][0]['distance'] / 1000
        duracion_seg = data['paths'][0]['time'] / 1000

        horas = int(duracion_seg // 3600)
        minutos = int((duracion_seg % 3600) // 60)
        segundos = int(duracion_seg % 60)

        print(f"\n✅ Distancia: {distancia_km:.2f} km")
        print(f"⏱️ Duración estimada: {horas}h {minutos}m {segundos}s\n")

    except Exception as e:
        print("❌ Error al realizar la consulta:", e)

# Bucle principal
while True:
    print("🔹 Ingrese ciudades (santiago, ovalle) — escriba 'q' para salir")
    origen = input("Ciudad de Origen: ").strip().lower()
    if origen == 'q':
        break
    destino = input("Ciudad de Destino: ").strip().lower()
    if destino == 'q':
        break

    obtener_datos_ruta(origen, destino)
