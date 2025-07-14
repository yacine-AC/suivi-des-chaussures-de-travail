'use client';
import React, { useState } from "react";
import { PieChart, Pie, Cell, Tooltip, Legend } from "recharts";

const initialData = [
  {
    name: "Khalid Aatif",
    cp: "7701945E",
    residence: "MTS",
    poste: "ADC",
    paire1: { modele: "Black Eagle Adventure", pointure: 45, recue: false },
    paire2: { modele: "Lavoro", pointure: 45, recue: false },
    remarques: "Non"
  },
  {
    name: "Mokrane Adnane",
    cp: "8712529R",
    residence: "MTS",
    poste: "ADC",
    paire1: { modele: "Black Eagle Athletic - Homme", pointure: 43, recue: false },
    paire2: { modele: "Lavoro", pointure: 43, recue: false },
    remarques: "Non"
  }
];

const COLORS = ["#0088FE", "#FF8042"];

export default function Page() {
  const [data, setData] = useState(initialData);

  const chartData = [
    {
      name: "Paires reçues",
      value: data.reduce((acc, d) => acc + (d.paire1.recue ? 1 : 0) + (d.paire2.recue ? 1 : 0), 0)
    },
    {
      name: "Paires manquantes",
      value: data.length * 2 - data.reduce((acc, d) => acc + (d.paire1.recue ? 1 : 0) + (d.paire2.recue ? 1 : 0), 0)
    }
  ];

  return (
    <div className="p-6 space-y-8">
      <h1 className="text-2xl font-bold">Suivi de Dotation Chaussures</h1>
      <div className="overflow-auto border rounded-md">
        <table className="min-w-full text-sm">
          <thead>
            <tr className="bg-gray-200">
              <th className="p-2">Nom</th>
              <th className="p-2">N° CP</th>
              <th className="p-2">Résidence</th>
              <th className="p-2">Poste</th>
              <th className="p-2">Paire 1</th>
              <th className="p-2">Reçue ?</th>
              <th className="p-2">Paire 2</th>
              <th className="p-2">Reçue ?</th>
              <th className="p-2">Remarques</th>
            </tr>
          </thead>
          <tbody>
            {data.map((agent, idx) => (
              <tr key={idx} className="border-t">
                <td className="p-2">{agent.name}</td>
                <td className="p-2">{agent.cp}</td>
                <td className="p-2">{agent.residence}</td>
                <td className="p-2">{agent.poste}</td>
                <td className="p-2">{agent.paire1.modele} ({agent.paire1.pointure})</td>
                <td className="p-2"><input type="checkbox" defaultChecked={agent.paire1.recue} /></td>
                <td className="p-2">{agent.paire2.modele} ({agent.paire2.pointure})</td>
                <td className="p-2"><input type="checkbox" defaultChecked={agent.paire2.recue} /></td>
                <td className="p-2">{agent.remarques}</td>
              </tr>
            ))}
          </tbody>
        </table>
      </div>

      <div className="w-[300px]">
        <PieChart width={300} height={200}>
          <Pie data={chartData} cx={150} cy={100} innerRadius={40} outerRadius={80} fill="#8884d8" dataKey="value" label>
            {chartData.map((entry, index) => (
              <Cell key={`cell-${index}`} fill={COLORS[index % COLORS.length]} />
            ))}
          </Pie>
          <Tooltip />
          <Legend />
        </PieChart>
      </div>
    </div>
  );
}
