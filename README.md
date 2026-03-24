import React, { useState } from 'react';
import { Shield, Search, FileText, Gavel, ArrowLeft, AlertCircle, CheckCircle, XCircle, Clock, Info, X, ChevronLeft } from 'lucide-react';

const processData = {
  inicial: {
    id: 'inicial',
    icon: Search,
    title: 'Investigación Inicial',
    objetivos: 'Iniciar el esclarecimiento de los hechos tras una denuncia para determinar si se cometió un delito.',
    sujetos: 'Ministerio Público (MP), víctima, asesor jurídico, imputado y defensor. Juez de Control (en audiencia inicial).',
    plazos: '72 horas, prorrogable a 144 horas constitucionales.',
    actosBase: [
      { 
        id: 'denuncia',
        title: 'Denuncia', 
        content: {
          accion: 'El MP recibe la noticia criminal e inicia la carpeta de investigación.',
          efecto: 'Se activan las facultades de investigación de la policía y peritos para recolectar indicios.'
        }
      },
      { 
        id: 'control',
        title: 'Control de la detención', 
        content: {
          analisis: 'El Juez revisa que no hayan pasado más de 48h desde la detención ante el MP y que se cumplan los requisitos de flagrancia (Art. 16 Const).',
          resultado: 'Si se ratifica, el sujeto permanece detenido para la imputación; si no, se decreta libertad inmediata.'
        }
      }
    ],
    actosPostImputacion: [
      'Declaración del imputado: Decide si contesta la imputación o guarda silencio.',
      'Vinculación a proceso: El juez decide si hay información suficiente para iniciar el proceso penal.',
      'Medidas cautelares: Solicitadas por el MP para garantizar la presencia del imputado o proteger a la víctima.'
    ]
  },
  complementaria: {
    id: 'complementaria',
    icon: Shield,
    title: 'Investigación Complementaria',
    objetivos: 'Permitir al Ministerio Público y a la defensa continuar con la recolección de pruebas para robustecer el caso tras la vinculación a proceso.',
    sujetos: 'Ministerio Público, defensa e imputado.',
    plazos: 'Cierre de investigación no mayor a seis meses.',
    actos: [
      {
        id: 'actos_inv',
        title: 'Realización de actos de investigación y registros...',
        content: {
          naturaleza: 'Fase de depuración y recolección de medios probatorios bajo control judicial.',
          acciones: 'Entrevistas a testigos, peritajes especializados (genética, balística, contabilidad), análisis de datos y solicitud de órdenes de cateo o intervención si es necesario.',
          finalidad: 'El Ministerio Público debe decidir, antes de que venza el plazo de seis meses, si tiene elementos suficientes para formular la Acusación o si solicita el Sobreseimiento (cierre) de la causa.'
        }
      }
    ]
  },
  intermedia: {
    id: 'intermedia',
    icon: FileText,
    title: 'Etapa Intermedia',
    objetivos: 'El ofrecimiento y admisión de los medios de prueba, así como la depuración de los hechos controvertidos.',
    sujetos: 'Juez de Control, Ministerio Público, víctima, asesor jurídico, imputado y defensor.',
    actosBase: [
      {
        id: 'escrito_acusacion',
        title: 'Escrito de Acusación',
        content: {
          definicion: 'Acto procesal mediante el cual el Ministerio Público formaliza la pretensión punitiva.',
          critico: 'Debe contener la individualización de los acusados, la relación circunstanciada de los hechos, la autoría o participación, y la lista de medios de prueba que se pretenden desahogar.',
          consecuencia: 'Marca el inicio de la fase escrita de la etapa intermedia.'
        }
      },
      {
        id: 'descubrimiento_probatorio',
        title: 'Descubrimiento Probatorio',
        content: {
          naturaleza: 'Es la obligación de las partes (MP y Defensa) de darse a conocer entre sí los medios de prueba que ofrecerán en la audiencia.',
          regla: 'El Ministerio Público no puede ocultar ninguna prueba, incluso si esta favorece al imputado (Principio de Lealtad).',
          efecto: 'Garantiza el equilibrio procesal y evita sorpresas en el Juicio Oral.'
        }
      }
    ],
    actosJuicio: [
      'Audiencia intermedia: El juez decide qué pruebas son válidas, lícitas y pertinentes.',
      'Auto de apertura a juicio oral: Acuerdo que dicta el cierre y ordena inicio del juicio.'
    ],
    actosSalida: [
      'Acuerdo Reparatorio / Suspensión Condicional / Procedimiento Abreviado: Mecanismos alternativos que extinguen la acción penal y evitan llegar a la etapa de Juicio Oral.'
    ]
  },
  juicio: {
    id: 'juicio',
    icon: Gavel,
    title: 'Juicio Oral',
    objetivos: 'Resolver la responsabilidad del imputado mediante el desahogo de pruebas ante un tribunal.',
    sujetos: 'Tribunal de enjuiciamiento, MP, defensa, imputado, víctima y testigos.',
    plazos: 'Máximo de dos años sin dictar sentencia. Máximo 10 días para apelar tras notificación.',
    actos: [
      'Audiencia de Juicio: Desahogo de pruebas y alegatos.',
      'Sentencia: Resolución que determina culpabilidad o inocencia.',
      'Audiencia de Individualización de la pena: Se discute la sanción en caso de condena.',
      'Audiencia de lectura y explicación: El tribunal explica fundamentos del fallo.'
    ]
  }
};

export default function App() {
  const [activeStage, setActiveStage] = useState(null);
  const [isDetentionLegal, setIsDetentionLegal] = useState(null);
  const [isSalidaAlterna, setIsSalidaAlterna] = useState(null);
  const [modalContent, setModalContent] = useState(null);

  const resetState = () => {
    setActiveStage(null);
    setIsDetentionLegal(null);
    setIsSalidaAlterna(null);
    setModalContent(null);
  };

  const renderHome = () => (
    <div className="flex flex-col items-center justify-center min-h-screen bg-slate-50 p-6 animate-in fade-in duration-500">
      <div className="max-w-4xl w-full">
        <h1 className="text-3xl font-light text-slate-800 mb-2 text-center tracking-tight">Arquitectura del Proceso Penal</h1>
        <p className="text-slate-500 text-center mb-12 text-sm uppercase tracking-widest">Selecciona un nodo para desplegar la estructura</p>
        
        <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
          {Object.values(processData).map((stage) => {
            const Icon = stage.icon;
            return (
              <button
                key={stage.id}
                onClick={() => setActiveStage(stage.id)}
                className="group flex flex-col items-center p-8 bg-white border border-slate-200 hover:border-slate-800 transition-all duration-300 shadow-sm hover:shadow-md"
              >
                <Icon className="w-8 h-8 text-slate-400 group-hover:text-slate-900 mb-4 transition-colors" />
                <h2 className="text-lg font-medium text-slate-700 group-hover:text-slate-900">{stage.title}</h2>
              </button>
            );
          })}
        </div>
      </div>
    </div>
  );

  const renderPlazos = (plazosText) => {
    const parts = plazosText.split(/(72 horas|144 horas|dos años|10 días|seis meses)/gi);
    return (
      <div className="inline-flex items-center gap-2 mt-2 bg-slate-900 text-slate-50 px-4 py-2 rounded-sm shadow-sm">
        <Clock className="w-4 h-4 text-slate-400" />
        <span className="text-sm tracking-wide">
          {parts.map((part, i) => 
            /(72 horas|144 horas|dos años|10 días|seis meses)/gi.test(part) ? 
            <strong key={i} className="font-mono text-base text-white">{part}</strong> : 
            <span key={i} className="text-slate-300">{part}</span>
          )}
        </span>
      </div>
    );
  };

  const renderModal = () => {
    if (!modalContent) return null;
    const currentStageName = processData[activeStage]?.title;

    return (
      <div className="fixed inset-0 bg-slate-900/60 backdrop-blur-md z-50 flex items-center justify-center p-4 animate-in fade-in duration-300">
        <div className="bg-white max-w-lg w-full shadow-2xl border border-slate-200 animate-in slide-in-from-bottom-4 duration-300 overflow-hidden">
          {/* Header del Modal con Navegación Contextual */}
          <div className="p-4 border-b border-slate-100 bg-slate-50 flex justify-between items-center">
            <button 
              onClick={() => setModalContent(null)}
              className="flex items-center text-[10px] font-bold text-slate-500 hover:text-slate-900 uppercase tracking-widest transition-colors"
            >
              <ChevronLeft className="w-3 h-3 mr-1" />
              Volver a {currentStageName}
            </button>
            <button 
              onClick={() => setModalContent(null)} 
              className="p-1 rounded-full hover:bg-slate-200 text-slate-400 hover:text-slate-900 transition-all"
              title="Cerrar descripción"
            >
              <X className="w-5 h-5" />
            </button>
          </div>

          <div className="p-8 space-y-6 max-h-[70vh] overflow-y-auto">
            <h4 className="text-xl font-bold text-slate-900 uppercase tracking-tight mb-2">{modalContent.title}</h4>

            {/* Aviso de Restricción Temporal para Inv. Complementaria */}
            {activeStage === 'complementaria' && (
              <div className="bg-slate-950 text-white p-4 flex items-center gap-3 border-l-4 border-slate-500 animate-pulse">
                <Clock className="w-5 h-5 text-slate-400" />
                <div>
                  <p className="text-[10px] uppercase tracking-[0.2em] font-bold text-slate-400">Restricción Temporal Crítica</p>
                  <p className="text-sm font-mono">PLAZO MÁXIMO: SEIS MESES</p>
                </div>
              </div>
            )}

            {/* Renderizado de contenidos dinámicos basados en la estructura técnica */}
            {modalContent.content.definicion && (
              <div className="space-y-2">
                <span className="text-xs font-bold text-slate-400 uppercase tracking-widest block">Definición Procesal</span>
                <p className="text-slate-700 leading-relaxed font-medium">{modalContent.content.definicion}</p>
              </div>
            )}
            {modalContent.content.naturaleza && (
              <div className="space-y-2">
                <span className="text-xs font-bold text-slate-400 uppercase tracking-widest block">Naturaleza Jurídica</span>
                <p className="text-slate-700 leading-relaxed font-medium">{modalContent.content.naturaleza}</p>
              </div>
            )}
            {modalContent.content.critico && (
              <div className="space-y-2">
                <span className="text-xs font-bold text-slate-400 uppercase tracking-widest block">Contenido Crítico</span>
                <p className="text-slate-700 leading-relaxed italic bg-slate-50 p-3 border-l-2 border-slate-300">{modalContent.content.critico}</p>
              </div>
            )}
            {modalContent.content.regla && (
              <div className="space-y-2">
                <span className="text-xs font-bold text-slate-400 uppercase tracking-widest block">Regla de Oro (Lealtad)</span>
                <p className="text-slate-700 leading-relaxed italic bg-slate-50 p-3 border-l-2 border-slate-300">{modalContent.content.regla}</p>
              </div>
            )}
            {modalContent.content.consecuencia && (
              <div className="space-y-2">
                <span className="text-xs font-bold text-slate-400 uppercase tracking-widest block">Consecuencia</span>
                <div className="p-4 bg-slate-900 text-slate-100 font-mono text-sm leading-relaxed">
                  {modalContent.content.consecuencia}
                </div>
              </div>
            )}
            {modalContent.content.efecto && (
              <div className="space-y-2">
                <span className="text-xs font-bold text-slate-400 uppercase tracking-widest block">Efecto Procesal</span>
                <div className="p-4 bg-slate-900 text-slate-100 font-mono text-sm leading-relaxed">
                  {modalContent.content.efecto}
                </div>
              </div>
            )}

            {/* Legado de Etapa Inicial */}
            {modalContent.content.accion && (
              <div className="space-y-2">
                <span className="text-xs font-bold text-slate-400 uppercase tracking-widest block">Acción</span>
                <p className="text-slate-700 leading-relaxed font-medium">{modalContent.content.accion}</p>
              </div>
            )}
          </div>
          <div className="p-4 bg-slate-50 text-right border-t border-slate-100">
            <button 
              onClick={() => setModalContent(null)}
              className="px-6 py-2 bg-slate-800 text-white text-xs font-bold uppercase tracking-widest hover:bg-slate-950 transition-colors shadow-sm"
            >
              Cerrar Vista
            </button>
          </div>
        </div>
      </div>
    );
  };

  const renderStageDetail = () => {
    const data = processData[activeStage];
    const Icon = data.icon;

    return (
      <div className="min-h-screen bg-white p-6 md:p-12 relative animate-in fade-in slide-in-from-right-4 duration-500">
        {renderModal()}
        <div className="max-w-3xl mx-auto">
          <button 
            onClick={resetState}
            className="flex items-center text-sm font-medium text-slate-400 hover:text-slate-900 mb-8 transition-colors uppercase tracking-wider group"
          >
            <ArrowLeft className="w-4 h-4 mr-2 group-hover:-translate-x-1 transition-transform" />
            Sistema Central
          </button>

          <div className="flex items-center gap-4 mb-8 pb-8 border-b border-slate-100">
            <div className="p-3 bg-slate-50 border border-slate-200">
              <Icon className="w-6 h-6 text-slate-800" />
            </div>
            <div>
              <h2 className="text-3xl font-light text-slate-900 tracking-tight">{data.title}</h2>
            </div>
          </div>

          <div className="space-y-10">
            <div className="grid grid-cols-1 md:grid-cols-2 gap-8">
              <section>
                <h3 className="text-xs font-bold text-slate-400 uppercase tracking-widest mb-3">Objetivos Principales</h3>
                <p className="text-slate-700 leading-relaxed">{data.objetivos}</p>
              </section>
              <section>
                <h3 className="text-xs font-bold text-slate-400 uppercase tracking-widest mb-3">Sujetos Procesales</h3>
                <p className="text-slate-700 leading-relaxed">{data.sujetos}</p>
              </section>
            </div>

            {data.plazos && (
              <section className="bg-slate-50 p-6 border-l-4 border-slate-900">
                <h3 className="text-xs font-bold text-slate-500 uppercase tracking-widest mb-2">Restricción Temporal</h3>
                {renderPlazos(data.plazos)}
              </section>
            )}

            <section>
              <h3 className="text-xs font-bold text-slate-400 uppercase tracking-widest mb-4">Actos Procesales Clave</h3>
              
              <div className="space-y-4">
                {(activeStage === 'inicial' || activeStage === 'complementaria' || activeStage === 'intermedia') ? (
                  (activeStage === 'inicial' ? data.actosBase : (activeStage === 'intermedia' ? data.actosBase : data.actos)).map((acto) => (
                    <button 
                      key={acto.id}
                      onClick={() => setModalContent(acto)}
                      className="w-full text-left p-4 border border-slate-200 hover:border-slate-800 bg-white group transition-all flex justify-between items-center hover:shadow-sm animate-in fade-in slide-in-from-bottom-2"
                    >
                      <span className="text-slate-700 group-hover:text-slate-900 font-medium">{acto.title}</span>
                      <div className="flex items-center gap-2 text-slate-400 group-hover:text-slate-900">
                        <span className="text-[10px] uppercase font-bold tracking-tighter">Detalles Técnicos</span>
                        <Info className="w-4 h-4" />
                      </div>
                    </button>
                  ))
                ) : null}

                {/* Lógica de Decisión Inicial */}
                {activeStage === 'inicial' && (
                  <div className="my-8 p-6 bg-slate-100 border border-slate-300">
                    <div className="flex items-center gap-3 mb-4">
                      <AlertCircle className="w-5 h-5 text-slate-700" />
                      <h4 className="font-semibold text-slate-900">Nodo de Decisión: ¿La detención fue legal?</h4>
                    </div>
                    <div className="flex gap-4">
                      <button onClick={() => setIsDetentionLegal(true)} className={`flex-1 py-3 px-4 border transition-all font-bold text-xs uppercase ${isDetentionLegal === true ? 'bg-slate-900 text-white border-slate-900' : 'bg-white text-slate-700 border-slate-300'}`}>SÍ</button>
                      <button onClick={() => setIsDetentionLegal(false)} className={`flex-1 py-3 px-4 border transition-all font-bold text-xs uppercase ${isDetentionLegal === false ? 'bg-red-900 text-white border-red-900' : 'bg-white text-slate-700 border-slate-300'}`}>NO</button>
                    </div>
                    {isDetentionLegal === true && (
                      <div className="mt-6 p-4 border-l-4 border-slate-800 bg-white animate-in slide-in-from-top-2 duration-300">
                        <h5 className="font-bold text-slate-900 mb-1">Ruta Activa: Judicialización</h5>
                        <div className="space-y-2 mt-4">
                          {data.actosPostImputacion.map((acto, idx) => (
                            <div key={idx} className="text-sm p-3 bg-slate-50 border border-slate-100 text-slate-700">{acto}</div>
                          ))}
                        </div>
                      </div>
                    )}
                    {isDetentionLegal === false && (
                      <div className="mt-6 p-6 border-l-4 border-red-600 bg-red-50 animate-in slide-in-from-top-2 duration-300">
                        <h5 className="font-bold text-red-700 text-xl tracking-tighter mb-1">LIBERTAD INMEDIATA</h5>
                        <p className="text-sm text-slate-700">Ilegalidad decretada. El sistema bloquea el avance procesal.</p>
                      </div>
                    )}
                  </div>
                )}

                {/* Lógica de Etapa Intermedia (Salidas Alternas) */}
                {activeStage === 'intermedia' && (
                  <div className="my-8 p-6 bg-slate-100 border border-slate-300">
                    <div className="flex items-center gap-3 mb-4">
                      <AlertCircle className="w-5 h-5 text-slate-700" />
                      <h4 className="font-semibold text-slate-900 uppercase text-xs tracking-widest">Opción: ¿Salida Alterna?</h4>
                    </div>
                    <div className="flex gap-4">
                      <button onClick={() => setIsSalidaAlterna(true)} className={`flex-1 py-3 px-4 border transition-all text-xs font-bold uppercase ${isSalidaAlterna === true ? 'bg-emerald-800 text-white border-emerald-800' : 'bg-white text-slate-700 border-slate-300'}`}>SÍ</button>
                      <button onClick={() => setIsSalidaAlterna(false)} className={`flex-1 py-3 px-4 border transition-all text-xs font-bold uppercase ${isSalidaAlterna === false ? 'bg-slate-900 text-white border-slate-900' : 'bg-white text-slate-700 border-slate-300'}`}>NO</button>
                    </div>
                    {isSalidaAlterna === true && (
                      <div className="mt-6 p-4 border-l-4 border-emerald-600 bg-emerald-50 animate-in slide-in-from-top-2 duration-300">
                        <div className="text-emerald-800
