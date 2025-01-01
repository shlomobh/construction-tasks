import React, { useState } from 'react';
import TaskList from './components/TaskList';
import TaskForm from './components/TaskForm';
import TaskDetails from './components/TaskDetails';
import DocumentUpload from './components/DocumentUpload';
import VoiceRecorder from './components/VoiceRecorder';
import TaskViews from './components/TaskViews';

const App = () => {
  const [showTaskDetails, setShowTaskDetails] = useState(false);
  const [selectedTask, setSelectedTask] = useState<any>(null);

  // נתוני דוגמה
  const mockTasks = [
    {
      id: '1',
      title: 'התקנת חלונות בקומה 5',
      description: 'התקנת חלונות אלומיניום בכל הדירות בקומה 5',
      category: 'אלומיניום',
      status: 'OPEN',
      assignee: 'יוסי כהן',
      dueDate: '2024-02-15',
      views: [
        { userId: '1', userName: 'ישראל ישראלי', timestamp: new Date() },
        { userId: '2', userName: 'משה כהן', timestamp: new Date() }
      ],
      documents: [
        { id: '1', name: 'מפרט טכני.pdf', url: '#' },
        { id: '2', name: 'תכנית התקנה.docx', url: '#' }
      ]
    }
  ];

  const handleSaveTask = (taskData: any) => {
    console.log('שמירת משימה:', taskData);
    setShowTaskDetails(false);
  };

  const handleVoiceTranscription = (text: string) => {
    if (selectedTask) {
      setSelectedTask({
        ...selectedTask,
        description: text
      });
    }
  };

  return (
    <div className="min-h-screen bg-gray-100" dir="rtl">
      {/* תפריט ניווט */}
      <nav className="bg-white shadow-sm fixed top-0 w-full z-50">
        <div className="max-w-7xl mx-auto px-4 py-3">
          <div className="flex justify-between items-center">
            <h1 className="text-xl font-bold">ניהול משימות בנייה</h1>
            <span className="text-sm text-gray-600">שלום, ישראל</span>
          </div>
        </div>
      </nav>

      {/* תוכן ראשי */}
      <main className="pt-16 pb-20">
        <div className="max-w-7xl mx-auto px-4 py-6">
          {/* כותרת וכפתור הוספה */}
          <div className="flex justify-between items-center mb-6">
            <h2 className="text-2xl font-bold">משימות פעילות</h2>
            <button 
              onClick={() => setShowTaskDetails(true)}
              className="bg-blue-600 text-white px-4 py-2 rounded-lg hover:bg-blue-700"
            >
              משימה חדשה +
            </button>
          </div>

          {/* רשימת משימות */}
          <TaskList 
            tasks={mockTasks}
            onTaskClick={(task) => {
              setSelectedTask(task);
              setShowTaskDetails(true);
            }}
          />
        </div>
      </main>

      {/* מודל פרטי משימה */}
      {showTaskDetails && (
        <div className="fixed inset-0 bg-black bg-opacity-50 z-50 overflow-y-auto">
          <div className="min-h-screen flex items-center justify-center p-4">
            <div className="bg-white rounded-lg w-full max-w-2xl">
              <div className="p-6 border-b flex justify-between items-start">
                <h2 className="text-xl font-bold">
                  {selectedTask ? 'עריכת משימה' : 'משימה חדשה'}
                </h2>
                <button 
                  onClick={() => {
                    setShowTaskDetails(false);
                    setSelectedTask(null);
                  }}
                  className="text-gray-500"
                >
                  ✕
                </button>
              </div>
              
              <div className="p-6">
                <TaskForm
                  initialData={selectedTask}
                  onSubmit={handleSaveTask}
                />

                {/* העלאת מסמכים */}
                <div className="mt-6">
                  <h3 className="text-lg font-medium mb-4">מסמכים מצורפים</h3>
                  <DocumentUpload 
                    taskId={selectedTask?.id}
                    existingDocuments={selectedTask?.documents || []}
                  />
                </div>

                {/* הקלטה קולית */}
                <div className="mt-6">
                  <h3 className="text-lg font-medium mb-4">הוספת תיאור בהקלטה</h3>
                  <VoiceRecorder onTranscription={handleVoiceTranscription} />
                </div>

                {/* צפיות במשימה */}
                {selectedTask && (
                  <TaskViews views={selectedTask.views || []} />
                )}
              </div>
            </div>
          </div>
        </div>
      )}
    </div>
  );
};

export default App;