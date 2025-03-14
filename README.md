import React, { useState } from 'react';
import { Card, CardContent } from '@/components/ui/card';
import { Button } from '@/components/ui/button';
import { Input } from '@/components/ui/input';
import { useNavigate } from 'react-router-dom';
import { LineChart, Line, XAxis, YAxis, CartesianGrid, Tooltip, Legend, ResponsiveContainer } from 'recharts';
import { Cpu, TrendingUp } from 'lucide-react';

// Bullex AI Home Component
function BullexAIHome() {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [mode, setMode] = useState('demo');
  const navigate = useNavigate();

  const handleLogin = () => {
    if (email && password) {
      console.log('Conectando à Bullex...', { email, password, mode });
      navigate('/dashboard', { state: { email, mode } });
    } else {
      alert('Preencha o e-mail e a senha.');
    }
  };

  return (
    <div className="min-h-screen flex items-center justify-center bg-gradient-to-r from-gray-900 to-gray-800 text-white p-4">
      <Card className="max-w-md w-full bg-gray-950 shadow-lg rounded-2xl p-6 border border-gray-700">
        <h1 className="text-2xl font-bold text-center mb-4 flex items-center justify-center gap-2">
          <Cpu size={24} /> Bullex AI
        </h1>
        <CardContent>
          <Input placeholder="E-mail" value={email} onChange={(e) => setEmail(e.target.value)} className="mb-4 bg-gray-800 border border-gray-700 text-white" />
          <Input type="password" placeholder="Senha" value={password} onChange={(e) => setPassword(e.target.value)} className="mb-4 bg-gray-800 border border-gray-700 text-white" />
          <div className="mb-4 flex justify-between">
            <Button variant={mode === 'demo' ? 'default' : 'outline'} onClick={() => setMode('demo')}>Conta Demo</Button>
            <Button variant={mode === 'real' ? 'default' : 'outline'} onClick={() => setMode('real')}>Conta Real</Button>
          </div>
          <Button className="w-full bg-green-500 hover:bg-green-600 text-white" onClick={handleLogin}>Entrar</Button>
        </CardContent>
      </Card>
    </div>
  );
}

// Robot Card Component
const RobotCard = ({ robot, onToggle }) => (
  <Card key={robot.id} className="bg-gray-950 shadow-md rounded-2xl p-4 border border-gray-700">
    <h2 className="text-lg font-semibold mb-2">{robot.name}</h2>
    <p className="mb-4">Status: {robot.status}</p>
    <Button className={`mb-4 ${robot.status === 'Parado' ? 'bg-green-500 hover:bg-green-600' : 'bg-red-500 hover:bg-red-600'} text-white`} onClick={() => onToggle(robot.id)}>
      {robot.status === 'Parado' ? 'Iniciar' : 'Parar'}
    </Button>
    <ResponsiveContainer width="100%" height={150}>
      <LineChart data={robot.data}>
        <CartesianGrid strokeDasharray="3 3" stroke="gray" />
        <XAxis dataKey="time" stroke="white" />
        <YAxis stroke="white" />
        <Tooltip contentStyle={{ backgroundColor: '#1f2937', color: 'white' }} />
        <Legend iconType="plainline" wrapperStyle={{ color: 'white' }} />
        <Line type="monotone" dataKey="profit" stroke="#10b981" strokeWidth={2} />
      </LineChart>
    </ResponsiveContainer>
  </Card>
);

// Dashboard Component
function Dashboard({ location }) {
  const { email, mode } = location.state || {};
  const [robots, setRobots] = useState([
    { id: 1, name: 'RSI Bot', status: 'Parado', data: [{ time: '10:00', profit: 10 }, { time: '10:05', profit: 15 }, { time: '10:10', profit: 20 }] },
    { id: 2, name: 'MACD Bot', status: 'Parado', data: [{ time: '10:00', profit: 5 }, { time: '10:05', profit: 12 }, { time: '10:10', profit: 18 }] },
    { id: 3, name: 'LTA/LTB Bot', status: 'Parado', data: [{ time: '10:00', profit: 8 }, { time: '10:05', profit: 14 }, { time: '10:10', profit: 22 }] },
    { id: 4, name: 'Scalper Bot', status: 'Parado', data: [{ time: '10:00', profit: 3 }, { time: '10:05', profit: 7 }, { time: '10:10', profit: 12 }] },
  ]);

  const handleToggleRobot = (id) => {
    setRobots((prev) => prev.map((robot) =>
      robot.id === id ? { ...robot, status: robot.status === 'Parado' ? 'Rodando' : 'Parado' } : robot
    ));
  };

  return (
    <div className="min-h-screen p-4 bg-gradient-to-r from-gray-900 to-gray-800 text-white">
      <h1 className="text-2xl font-bold text-center mb-4 flex items-center justify-center gap-2">
        <TrendingUp size={28} /> Painel de Controle
      </h1>
      <p className="text-center mb-6">Conectado como: {email} | Modo: {mode === 'demo' ? 'Conta Demo' : 'Conta Real'}</p>
      <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
        {robots.map((robot) => (
          <RobotCard key={robot.id} robot={robot} onToggle={handleToggleRobot} />
        ))}
      </div>
    </div>
  );
}

export { BullexAIHome, Dashboard };
