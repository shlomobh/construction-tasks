import React, { useState } from 'react';

const App = () => {
  const [showTaskDetails, setShowTaskDetails] = useState(false);
  
  // נתוני דוגמה למשימות
  const mockTasks = [
    {
      id: '1',
      title: 'התקנת חלונות בקומה 5',
      description: 'התקנת חלונות אלומיניום בכל הדירות בקומה 5',
      category: 'אלומיניום',
      status: 'פתוח',
      assignee: 'יוסי כהן',
      dueDate: '2024-02-15'
    },
    {
      id: '2',
      title: 'ריצוף לובי כניסה',
      description: 'ריצוף שיש בלובי הכניסה',
      category: 'ריצוף',
      status: 'בביצוע',
      assignee: 'משה דוד',
      dueDate: '2024-02-20'
    }
  ];

  return (
    <div className="min-h-screen bg-gray-100" dir="rtl">
      {/* כותרת */}
      <header className="bg-white shadow-sm">
        <div className="max-w-7xl mx-auto px-4 py-4">
          <h1 className="text-xl font-bold">ניהול משימות בנייה</h1>
        </div>
      </header>

      {/* תוכן ראשי */}
      <main className="max-w-7xl mx-auto px-4 py-6">
        {/* כותרת ראשית */}
        <div className="flex justify-between items-center mb-6">
          <h2 className="text-lg font-semibold">משימות פעילות</h2>
          <button 
            onClick={() => setShowTaskDetails(true)}
            className="bg-blue-600 text-white px-4 py-2 rounded-lg hover:bg-blue-700"
          >
            משימה חדשה +
          </button>
        </div>

        {/* רשימת משימות */}
        <div className="grid gap-4">
          {mockTasks.map(task => (
            <div 
              key={task.id}
              className="bg-white rounded-lg shadow p-4"
              onClick={() => setShowTaskDetails(true)}
            >
              <h3 className="font-medium mb-2">{task.title}</h3>
              <p className="text-sm text-gray-600 mb-2">{task.description}</p>
              <div className="flex justify-between items-center text-sm">
                <span>{task.assignee}</span>
                <span className="px-2 py-1 bg-blue-100 text-blue-800 rounded-full">
                  {task.status}
                </span>
              </div>
            </div>
          ))}
        </div>
      </main>

      {/* מודל פרטי משימה */}
      {showTaskDetails && (
        <div className="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center p-4">
          <div className="bg-white rounded-lg w-full max-w-md">
            <div className="p-6 border-b">
              <div className="flex justify-between items-start">
                <h2 className="text-lg font-semibold">פרטי משימה</h2>
                <button 
                  onClick={() => setShowTaskDetails(false)}
                  className="text-gray-500"
                >
                  ✕
                </button>
              </div>
            </div>
            <div className="p-6">
              <div className="space-y-4">
                <div>
                  <label className="block text-sm font-medium mb-1">כותרת</label>
                  <input 
                    type="text"
                    className="w-full p-2 border rounded-lg"
                  />
                </div>
                <div>
                  <label className="block text-sm font-medium mb-1">תיאור</label>
                  <textarea 
                    className="w-full p-2 border rounded-lg"
                    rows={4}
                  />
                </div>
                <div className="grid grid-cols-2 gap-4">
                  <div>
                    <label className="block text-sm font-medium mb-1">אחראי</label>
                    <input 
                      type="text"
                      className="w-full p-2 border rounded-lg"
                    />
                  </div>
                  <div>
                    <label className="block text-sm font-medium mb-1">תאריך יעד</label>
                    <input 
                      type="date"
                      className="w-full p-2 border rounded-lg"
                    />
                  </div>
                </div>
                <button className="w-full bg-blue-600 text-white p-2 rounded-lg hover:bg-blue-700">
                  שמור משימה
                </button>
              </div>
            </div>
          </div>
        </div>
      )}
    </div>
  );
};

export default App;
