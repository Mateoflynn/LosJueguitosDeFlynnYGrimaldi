"use client"

import { useState, useEffect } from "react"
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card"
import { Button } from "@/components/ui/button"
import { ChevronLeft, ChevronRight } from 'lucide-react'

// Datos de ejemplo basados en tu archivo - CON ICONOS
const genreData = [
  { genre: "Acción", count: 34, color: "#ef4444", icon: "⚔️" },
  { genre: "Aventura", count: 21, color: "#22c55e", icon: "🗺️" },
  { genre: "RPG", count: 15, color: "#8b5cf6", icon: "🐉" },
  { genre: "Estrategia", count: 7, color: "#06b6d4", icon: "♟️" },
  { genre: "Puzzle", count: 6, color: "#ec4899", icon: "🧩" },
  { genre: "Simulación", count: 5, color: "#10b981", icon: "🏗️" },
  { genre: "Lucha", count: 5, color: "#dc2626", icon: "👊" },
  { genre: "Carreras", count: 4, color: "#3b82f6", icon: "🏎️" },
  { genre: "Deportes", count: 3, color: "#f59e0b", icon: "⚽" },
]

const regionalData = [
  { region: "North America", count: 44, color: "#3b82f6", shortName: "NA" },
  { region: "Asia", count: 40, color: "#ef4444", shortName: "Asia" },
  { region: "Europe", count: 15, color: "#22c55e", shortName: "EU" },
  { region: "Oceania", count: 1, color: "#f59e0b", shortName: "OCE" },
]

export default function ImprovedCharts() {
  const [isVisible, setIsVisible] = useState(false)
  const [hoveredGenre, setHoveredGenre] = useState<string | null>(null)
  const [hoveredRegion, setHoveredRegion] = useState<string | null>(null)
  const [currentPage, setCurrentPage] = useState(0)

  useEffect(() => {
    const timer = setTimeout(() => setIsVisible(true), 300)
    return () => clearTimeout(timer)
  }, [])

  const total = genreData.reduce((sum, item) => sum + item.count, 0)

  // Dividir géneros en páginas de 6 (2x3)
  const genresPerPage = 6
  const totalPages = Math.ceil(genreData.length / genresPerPage)
  const currentGenres = genreData
    .sort((a, b) => b.count - a.count)
    .slice(currentPage * genresPerPage, (currentPage + 1) * genresPerPage)

  const nextPage = () => {
    setCurrentPage((prev) => (prev + 1) % totalPages)
  }

  const prevPage = () => {
    setCurrentPage((prev) => (prev - 1 + totalPages) % totalPages)
  }

  return (
    <div className="min-h-screen bg-gradient-to-br from-slate-900 via-purple-900 to-slate-900 p-6">
      <div className="max-w-7xl mx-auto">
        {/* Header */}
        <div className="text-center mb-12">
          <h1 className="text-5xl font-bold bg-gradient-to-r from-purple-400 via-pink-400 to-cyan-400 bg-clip-text text-transparent mb-4">
            Análisis Visual de Datos
          </h1>
          <p className="text-xl text-gray-300 max-w-3xl mx-auto">
            Visualizaciones modernas e interactivas de nuestra colección de 100 juegos
          </p>
        </div>

        <div className="grid grid-cols-1 lg:grid-cols-2 gap-8 mb-12">
          {/* Gráfico de Géneros - CON ICONOS */}
          <Card className="bg-gray-900/50 border-gray-700 backdrop-blur-sm">
            <CardHeader className="flex flex-row items-center justify-between">
              <CardTitle className="text-2xl font-bold text-white flex items-center gap-3">
                <div className="w-3 h-3 bg-gradient-to-r from-purple-500 to-pink-500 rounded-full"></div>
                Distribución por Género
              </CardTitle>
              <div className="flex items-center gap-2">
                <Button
                  variant="ghost"
                  size="sm"
                  onClick={prevPage}
                  className="text-gray-400 hover:text-white hover:bg-gray-800"
                  disabled={totalPages <= 1}
                >
                  <ChevronLeft className="w-4 h-4" />
                </Button>
                <span className="text-gray-400 text-sm px-2">
                  {currentPage + 1} / {totalPages}
                </span>
                <Button
                  variant="ghost"
                  size="sm"
                  onClick={nextPage}
                  className="text-gray-400 hover:text-white hover:bg-gray-800"
                  disabled={totalPages <= 1}
                >
                  <ChevronRight className="w-4 h-4" />
                </Button>
              </div>
            </CardHeader>
            <CardContent className="p-6">
              <div className="relative">
                {/* Grid 2x3 de géneros CON ICONOS */}
                <div className="grid grid-cols-3 grid-rows-2 gap-4 mb-6 h-80">
                  {currentGenres.map((genre, index) => {
                    const percentage = (genre.count / total) * 100
                    const isHovered = hoveredGenre === genre.genre

                    return (
                      <div
                        key={genre.genre}
                        className="relative group cursor-pointer"
                        onMouseEnter={() => setHoveredGenre(genre.genre)}
                        onMouseLeave={() => setHoveredGenre(null)}
                      >
                        {/* Tarjeta principal */}
                        <div
                          className="relative p-4 rounded-xl border-2 transition-all duration-500 overflow-hidden h-full flex flex-col justify-center"
                          style={{
                            backgroundColor: `${genre.color}10`,
                            borderColor: isHovered ? genre.color : `${genre.color}30`,
                            transform: isHovered ? "scale(1.05) translateY(-3px)" : "scale(1)",
                            boxShadow: isHovered
                              ? `0 15px 30px ${genre.color}30, 0 0 20px ${genre.color}20`
                              : `0 3px 10px ${genre.color}20`,
                          }}
                        >
                          {/* Fondo animado */}
                          <div
                            className="absolute inset-0 opacity-0 group-hover:opacity-20 transition-opacity duration-500"
                            style={{
                              background: `radial-gradient(circle at center, ${genre.color}, transparent)`,
                            }}
                          />

                          {/* Contenido */}
                          <div className="relative z-10 text-center">
                            {/* ICONO DEL GÉNERO */}
                            <div className="text-3xl mb-3 transform transition-transform duration-300 group-hover:scale-110">
                              {genre.icon}
                            </div>

                            {/* Número principal */}
                            <div
                              className="text-2xl font-bold mb-2 transition-colors duration-300"
                              style={{ color: isHovered ? genre.color : "#ffffff" }}
                            >
                              {genre.count}
                            </div>

                            {/* Nombre del género */}
                            <div className="text-gray-200 font-medium text-sm mb-3">{genre.genre}</div>

                            {/* Barra de progreso */}
                            <div className="w-full h-2 bg-gray-800 rounded-full overflow-hidden">
                              <div
                                className="h-full rounded-full transition-all duration-1000 ease-out"
                                style={{
                                  width: isVisible ? `${percentage}%` : "0%",
                                  backgroundColor: genre.color,
                                  boxShadow: `0 0 8px ${genre.color}60`,
                                }}
                              />
                            </div>

                            {/* Porcentaje */}
                            <div className="text-gray-400 text-xs mt-2">{percentage.toFixed(1)}%</div>
                          </div>

                          {/* Efecto de partículas */}
                          {isHovered && (
                            <div className="absolute inset-0 pointer-events-none">
                              <div
                                className="absolute top-2 right-2 w-2 h-2 rounded-full animate-ping"
                                style={{ backgroundColor: genre.color }}
                              />
                              <div
                                className="absolute bottom-2 left-2 w-1 h-1 rounded-full animate-pulse"
                                style={{ backgroundColor: genre.color }}
                              />
                            </div>
                          )}
                        </div>

                        {/* Tooltip flotante */}
                        {isHovered && (
                          <div className="absolute -top-12 left-1/2 transform -translate-x-1/2 bg-gray-900 text-white px-3 py-1.5 rounded-lg text-xs whitespace-nowrap border border-gray-600 shadow-xl z-20">
                            <div className="font-semibold flex items-center gap-2">
                              <span>{genre.icon}</span>
                              {genre.genre}
                            </div>
                            <div className="text-gray-300">
                              {genre.count} juegos ({percentage.toFixed(1)}%)
                            </div>
                            <div className="absolute top-full left-1/2 transform -translate-x-1/2 w-0 h-0 border-l-2 border-r-2 border-t-2 border-transparent border-t-gray-900"></div>
                          </div>
                        )}
                      </div>
                    )
                  })}
                </div>

                {/* Indicadores de página */}
                <div className="flex justify-center gap-2 mb-4">
                  {Array.from({ length: totalPages }).map((_, index) => (
                    <button
                      key={index}
                      onClick={() => setCurrentPage(index)}
                      className={`w-2 h-2 rounded-full transition-all duration-300 ${
                        index === currentPage ? "bg-purple-500 w-6" : "bg-gray-600 hover:bg-gray-500"
                      }`}
                    />
                  ))}
                </div>

                {/* Estadísticas mejoradas */}
                <div className="grid grid-cols-3 gap-3">
                  <div className="p-3 bg-gradient-to-br from-purple-900/30 to-purple-800/30 rounded-lg border border-purple-700">
                    <div className="text-center">
                      <div className="text-xl font-bold text-purple-400 mb-1">{total}</div>
                      <div className="text-xs text-gray-400">Total</div>
                    </div>
                  </div>
                  <div className="p-3 bg-gradient-to-br from-pink-900/30 to-pink-800/30 rounded-lg border border-pink-700">
                    <div className="text-center">
                      <div className="text-xl font-bold text-pink-400 mb-1">{genreData.length}</div>
                      <div className="text-xs text-gray-400">Géneros</div>
                    </div>
                  </div>
                  <div className="p-3 bg-gradient-to-br from-cyan-900/30 to-cyan-800/30 rounded-lg border border-cyan-700">
                    <div className="text-center">
                      <div className="text-xl font-bold text-cyan-400 mb-1">{genreData[0]?.count || 0}</div>
                      <div className="text-xs text-gray-400">Top</div>
                    </div>
                  </div>
                </div>
              </div>
            </CardContent>
          </Card>

          {/* Gráfico Regional - SIN SÍMBOLOS (como estaba) */}
          <Card className="bg-gray-900/50 border-gray-700 backdrop-blur-sm">
            <CardHeader>
              <CardTitle className="text-2xl font-bold text-white flex items-center gap-3">
                <div className="w-3 h-3 bg-gradient-to-r from-cyan-500 to-blue-500 rounded-full"></div>
                Distribución Regional
              </CardTitle>
            </CardHeader>
            <CardContent className="p-6">
              <div className="space-y-6">
                {/* Gráfico de barras horizontales limpio */}
                <div className="space-y-4">
                  {regionalData
                    .sort((a, b) => b.count - a.count)
                    .map((region, index) => {
                      const percentage = (region.count / regionalData.reduce((sum, r) => sum + r.count, 0)) * 100
                      const isHovered = hoveredRegion === region.region

                      return (
                        <div
                          key={region.region}
                          className="group cursor-pointer"
                          onMouseEnter={() => setHoveredRegion(region.region)}
                          onMouseLeave={() => setHoveredRegion(null)}
                        >
                          {/* Encabezado de la región */}
                          <div className="flex items-center justify-between mb-2">
                            <div className="flex items-center gap-3">
                              <div className="w-4 h-4 rounded-full" style={{ backgroundColor: region.color }} />
                              <span className="text-gray-200 font-medium">{region.region}</span>
                            </div>
                            <div className="flex items-center gap-3">
                              <span className="text-gray-400 text-sm">{percentage.toFixed(1)}%</span>
                              <div
                                className="px-3 py-1 rounded-full text-sm font-bold"
                                style={{
                                  backgroundColor: `${region.color}20`,
                                  color: region.color,
                                  border: `1px solid ${region.color}40`,
                                }}
                              >
                                {region.count}
                              </div>
                            </div>
                          </div>

                          {/* Barra horizontal */}
                          <div className="w-full h-4 bg-gray-800 rounded-full overflow-hidden">
                            <div
                              className="h-full rounded-full transition-all duration-1000 ease-out relative"
                              style={{
                                width: isVisible ? `${percentage}%` : "0%",
                                background: `linear-gradient(90deg, ${region.color}, ${region.color}80)`,
                                boxShadow: isHovered ? `0 0 20px ${region.color}60` : `0 0 10px ${region.color}30`,
                                transform: isHovered ? "scaleY(1.2)" : "scaleY(1)",
                              }}
                            >
                              {isHovered && <div className="absolute inset-0 bg-white/20 rounded-full animate-pulse" />}
                            </div>
                          </div>
                        </div>
                      )
                    })}
                </div>

                {/* Estadísticas resumidas */}
                <div className="grid grid-cols-2 gap-4 mt-8">
                  <div className="p-4 bg-gray-800/30 rounded-lg border border-gray-700">
                    <div className="text-center">
                      <div className="text-2xl font-bold text-blue-400 mb-1">
                        {regionalData.sort((a, b) => b.count - a.count)[0]?.count || 0}
                      </div>
                      <div className="text-xs text-gray-400">Líder Regional</div>
                      <div className="text-xs text-gray-500">
                        {regionalData.sort((a, b) => b.count - a.count)[0]?.region || "N/A"}
                      </div>
                    </div>
                  </div>
                  <div className="p-4 bg-gray-800/30 rounded-lg border border-gray-700">
                    <div className="text-center">
                      <div className="text-2xl font-bold text-green-400 mb-1">{regionalData.length}</div>
                      <div className="text-xs text-gray-400">Regiones</div>
                      <div className="text-xs text-gray-500">Activas</div>
                    </div>
                  </div>
                </div>
              </div>
            </CardContent>
          </Card>
        </div>

        {/* Gráfico de comparación adicional */}
        <Card className="bg-gray-900/50 border-gray-700 backdrop-blur-sm">
          <CardHeader>
            <CardTitle className="text-2xl font-bold text-white flex items-center gap-3">
              <div className="w-3 h-3 bg-gradient-to-r from-yellow-500 to-orange-500 rounded-full"></div>
              Análisis Comparativo
            </CardTitle>
          </CardHeader>
          <CardContent className="p-6">
            <div className="grid grid-cols-1 md:grid-cols-3 gap-6">
              <div className="text-center p-6 bg-gradient-to-br from-purple-900/30 to-purple-800/30 rounded-lg border border-purple-700">
                <div className="text-4xl font-bold text-purple-400 mb-2">
                  {genreData.find((g) => g.genre === "Acción")?.count || 0}
                </div>
                <div className="text-gray-300 font-medium">Juegos de Acción</div>
                <div className="text-gray-400 text-sm mt-1">Género dominante</div>
              </div>

              <div className="text-center p-6 bg-gradient-to-br from-blue-900/30 to-blue-800/30 rounded-lg border border-blue-700">
                <div className="text-4xl font-bold text-blue-400 mb-2">
                  {regionalData.find((r) => r.region === "North America")?.count || 0}
                </div>
                <div className="text-gray-300 font-medium">Norteamérica</div>
                <div className="text-gray-400 text-sm mt-1">Región líder</div>
              </div>

              <div className="text-center p-6 bg-gradient-to-br from-green-900/30 to-green-800/30 rounded-lg border border-green-700">
                <div className="text-4xl font-bold text-green-400 mb-2">
                  {(((genreData.find((g) => g.genre === "Acción")?.count || 0) / total) * 100).toFixed(0)}%
                </div>
                <div className="text-gray-300 font-medium">Dominancia</div>
                <div className="text-gray-400 text-sm mt-1">Del género principal</div>
              </div>
            </div>
          </CardContent>
        </Card>
      </div>
    </div>
  )
}