import React, { useState } from 'react';
import Table from '@/components/Table';
import LineChart from '@/components/LineChart';
import BarChart from '@/components/BarChart';
import PieChart from '@/components/PieChart';

const chartTypes = ['line', 'bar', 'pie'] as const;
type ChartType = typeof chartTypes[number];

const Dashboard: React.FC = () => {
  const [selectedChart, setSelectedChart] = useState<ChartType>('bar');
  const [showDescription, setShowDescription] = useState(true);

  const columns = ['Name', 'Score', 'Remarks'];
  const data = [
    { Name: 'Fahim', Score: 85, Remarks: 'Excellent progress' },
    { Name: 'Ayesha', Score: 72, Remarks: 'Needs consistency' },
    { Name: 'Sam', Score: 92, Remarks: 'Outstanding' },
  ];

  const chartData = [
    { label: 'Jan', value: 30 },
    { label: 'Feb', value: 50 },
    { label: 'Mar', value: 80 },
    { label: 'Apr', value: 60 },
  ];

  const renderChart = () => {
    switch (selectedChart) {
      case 'line':
        return <LineChart data={chartData} title="Line Chart" />;
      case 'bar':
        return <BarChart data={chartData} title="Bar Chart" />;
      case 'pie':
        return <PieChart data={chartData} title="Pie Chart" />;
    }
  };

  return (
    <div className="min-h-screen bg-black text-white p-6 font-sans space-y-6">
      {/* Header */}
      <div className="flex justify-between items-center">
        <h1 className="text-2xl font-bold tracking-tight">📊 Dashboard Overview</h1>
        <div className="flex gap-2">
          {chartTypes.map((type) => (
            <button
              key={type}
              onClick={() => setSelectedChart(type)}
              className={`px-3 py-1 rounded-full text-sm font-medium border border-white transition ${
                selectedChart === type ? 'bg-white text-black' : 'text-white hover:bg-white hover:text-black'
              }`}
            >
              {type.toUpperCase()}
            </button>
          ))}
          <button
            onClick={() => setShowDescription(!showDescription)}
            className="px-3 py-1 rounded-full text-sm font-medium border border-white hover:bg-white hover:text-black transition"
          >
            {showDescription ? 'Hide Info' : 'Show Info'}
          </button>
        </div>
      </div>

      {/* Grid Layout */}
      <div className="grid md:grid-cols-2 gap-6">
        {/* Chart */}
        <div className="bg-white text-black p-4 rounded-xl shadow-md">{renderChart()}</div>

        {/* Description / Reasoning */}
        {showDescription && (
          <div className="bg-zinc-900 p-4 rounded-xl shadow-md text-sm leading-relaxed text-white border border-zinc-700">
            <h2 className="text-lg font-semibold mb-2">📌 Reasoning & Description</h2>
            <p>
              This dashboard presents performance and trend data with flexible views using different chart types. It enables users
              to switch between visualizations for better interpretation of patterns and anomalies. The table complements the
              visual data with detailed textual records.
            </p>
          </div>
        )}
      </div>

      {/* Table Section */}
      <div className="bg-white text-black p-4 rounded-xl shadow-md">
        <h2 className="text-lg font-semibold mb-2">📋 Detailed Data Table</h2>
        <Table
          columns={columns}
          data={data}
          headerClass="bg-black text-white"
          rowClassFn={(row, i) => (i % 2 === 0 ? 'bg-gray-100' : 'bg-white')}
          cellClassFn={() => 'text-center'}
        />
      </div>
    </div>
  );
};

export default Dashboard;
