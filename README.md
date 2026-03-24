<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Arquitectura del Proceso Penal</title>
    <script src="https://unpkg.com/react@18/umd/react.production.min.js"></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Inter', sans-serif; background-color: #f9fafb; }
        .card { transition: all 0.3s ease; }
        .card:hover { transform: translateY(-5px); box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1); }
    </style>
</head>
<body>
    <div id="root"></div>

    <script type="text/babel">
        const { useState } = React;

        const App = () => {
            const [view, setView] = useState('menu');

            return (
                <div className="min-h-screen p-4 md:p-8">
                    <header className="mb-8 text-center">
                        <h1 className="text-2xl font-bold text-gray-800">Arquitectura del Proceso Penal</h1>
                        <p className="text-gray-500 text-sm">SISTEMA DE CONSULTA INTERACTIVO</p>
                    </header>

                    {view === 'menu' && (
                        <div className="grid gap-4 max-w-md mx-auto">
                            <div onClick={() => setView('inicial')} className="card bg-white p-6 rounded-xl border border-gray-200 cursor-pointer flex items-center space-x-4">
                                <div className="bg-blue-100 p-3 rounded-full text-blue-600">🔍</div>
                                <div>
                                    <h2 className="font-semibold text-gray-800">Investigación Inicial</h2>
                                    <p className="text-xs text-gray-400">72 a 144 horas</p>
                                </div>
                            </div>
                            {/* Otros nodos se pueden agregar aquí */}
                        </div>
                    )}

                    {view === 'inicial' && (
                        <div className="max-w-lg mx-auto bg-white rounded-2xl shadow-sm border border-gray-100 overflow-hidden">
                            <div className="p-4 border-b flex items-center justify-between">
                                <button onClick={() => setView('menu')} className="text-gray-500 text-sm font-medium">← Volver</button>
                                <span className="font-bold text-gray-700">Investigación Inicial</span>
                                <div className="w-8"></div>
                            </div>
                            <div className="p-6">
                                <h3 className="text-xs font-bold text-gray-400 uppercase tracking-wider mb-2">Objetivos</h3>
                                <p className="text-gray-700 text-sm mb-6">Esclarecer los hechos tras una denuncia para determinar si se cometió un delito.</p>
                                
                                <div className="bg-black text-white p-4 rounded-lg mb-6">
                                    <span className="text-xs text-gray-400 block mb-1">RESTRICCIÓN TEMPORAL</span>
                                    <span className="text-lg font-bold">72 horas <span className="text-gray-500 font-normal">prorrogable a</span> 144 horas</span>
                                </div>

                                <div className="space-y-4">
                                    <div className="p-4 bg-gray-50 rounded-xl border border-gray-200">
                                        <h4 className="font-bold text-gray-800 text-sm">Denuncia</h4>
                                        <p className="text-xs text-gray-600 mt-1">El MP recibe la noticia criminal e inicia la carpeta.</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    )}
                </div>
            );
        };

        const root = ReactDOM.createRoot(document.getElementById('root'));
        root.render(<App />);
    </script>
</body>
</html>
