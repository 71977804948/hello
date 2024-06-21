import requests
from websocket import create_connection
import json
import matplotlib.pyplot as plt
import matplotlib.animation as animation

# Función para obtener datos de transacciones en tiempo real
def obtener_datos_xrp():
    url = 'https://api.binance.com/api/v3/trades'
    params = {
        'symbol': 'XRPUSDT',
        'limit': 1000  # Cantidad máxima de transacciones a obtener
    }
    response = requests.get(url, params=params)
    trades = response.json()
    return trades

# Función para visualizar datos en tiempo real
def visualizar_datos(trades):
    precios = [float(trade['price']) for trade in trades]
    tiempos = [trade['time'] for trade in trades]
    
    fig, ax = plt.subplots()
    line, = ax.plot([], [])

    def actualizar(i):
        line.set_data(tiempos[:i], precios[:i])
        ax.relim()
        ax.autoscale_view()
        return line,

    ani = animation.FuncAnimation(fig, actualizar, frames=len(trades), interval=1000)
    plt.title('Transacciones en tiempo real de XRP/USDT en Binance')
    plt.xlabel('Tiempo')
    plt.ylabel('Precio (USDT)')
    plt.show()

# Función principal
def main():
    trades = obtener_datos_xrp()
    visualizar_datos(trades)

if __name__ == "__main__":
    main()
