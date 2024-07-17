# Web-developer-
npx create-next-app@latest admin-dashboard
cd admin-dashboard
npm install @nextui-org/react @shadcn/ui @emotion/react @emotion/styled
mkdir components pages
import { useState } from 'react';

const UserList = ({ users, onEdit, onDelete }) => (
  <div>
    <h2>User List</h2>
    <ul>
      {users.map(user => (
        <li key={user.id}>
          {user.name} 
          <button onClick={() => onEdit(user)}>Edit</button>
          <button onClick={() => onDelete(user.id)}>Delete</button>
        </li>
      ))}
    </ul>
  </div>
);

export default UserList;
import { useState } from 'react';

const UserForm = ({ onSubmit, initialData }) => {
  const [name, setName] = useState(initialData ? initialData.name : '');

  const handleSubmit = (e) => {
    e.preventDefault();
    onSubmit({ id: initialData ? initialData.id : Date.now(), name });
    setName('');
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        value={name}
        onChange={(e) => setName(e.target.value)}
        placeholder="User Name"
        required
      />
      <button type="submit">{initialData ? 'Update' : 'Add'} User</button>
    </form>
  );
};

export default UserForm;
import { useState } from 'react';
import UserList from '../components/UserList';
import UserForm from '../components/UserForm';

const Home = () => {
  const [users, setUsers] = useState([]);
  const [editingUser, setEditingUser] = useState(null);

  const addUser = (user) => {
    setUsers([...users, user]);
  };

  const editUser = (user) => {
    setUsers(users.map(u => u.id === user.id ? user : u));
    setEditingUser(null);
  };

  const deleteUser = (id) => {
    setUsers(users.filter(user => user.id !== id));
  };

  return (
    <div>
      <UserForm onSubmit={editingUser ? editUser : addUser} initialData={editingUser} />
      <UserList users={users} onEdit={setEditingUser} onDelete={deleteUser} />
    </div>
  );
};

export default Home;
const Analytics = ({ userCount, activeUsers, revenue }) => (
  <div>
    <h2>Analytics</h2>
    <p>User Count: {userCount}</p>
    <p>Active Users: {activeUsers}</p>
    <p>Revenue: ${revenue}</p>
  </div>
);

export default Analytics;
import { useState } from 'react';

const Notifications = () => {
  const [notifications, setNotifications] = useState([]);

  const sendNotification = () => {
    const newNotification = {
      id: Date.now(),
      message: 'This is a test notification',
    };
    setNotifications([...notifications, newNotification]);
    // Trigger the PWA notification here
    if ("serviceWorker" in navigator) {
      navigator.serviceWorker.ready.then(registration => {
        registration.showNotification(newNotification.message);
      });
    }
  };

  return (
    <div>
      <h2>Notifications</h2>
      <button onClick={sendNotification}>Send Notification</button>
      <ul>
        {notifications.map(notification => (
          <li key={notification.id}>{notification.message}</li>
        ))}
      </ul>
    </div>
  );
};

export default Notifications;
import { useState } from 'react';

const Settings = ({ initialSettings, onSave }) => {
  const [settings, setSettings] = useState(initialSettings);

  const handleChange = (e) => {
    setSettings({ ...settings, [e.target.name]: e.target.value });
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    onSave(settings);
  };

  return (
    <form onSubmit={handleSubmit}>
      <label>
        App Name:
        <input
          type="text"
          name="appName"
          value={settings.appName}
          onChange={handleChange}
        />
      </label>
      <button type="submit">Save Settings</button>
    </form>
  );
};

export default Settings;
import { useState } from 'react';

const TaskList = ({ tasks, onEdit, onDelete }) => (
  <div>
    <h2>Task List</h2>
    <ul>
      {tasks.map(task => (
        <li key={task.id}>
          {task.title} 
          <button onClick={() => onEdit(task)}>Edit</button>
          <button onClick={() => onDelete(task.id)}>Delete</button>
        </li>
      ))}
    </ul>
  </div>
);

export default TaskList;
import { useState } from 'react';

const TaskForm = ({ onSubmit, initialData }) => {
  const [title, setTitle] = useState(initialData ? initialData.title : '');

  const handleSubmit = (e) => {
    e.preventDefault();
    onSubmit({ id: initialData ? initialData.id : Date.now(), title });
    setTitle('');
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        value={title}
        onChange={(e) => setTitle(e.target.value)}
        placeholder="Task Title"
        required
      />
      <button type="submit">{initialData ? 'Update' : 'Add'} Task</button>
    </form>
  );
};

export default TaskForm;
import { useState } from 'react';
import TaskList from '../components/TaskList';
import TaskForm from '../components/TaskForm';

const Tasks = () => {
  const [tasks, setTasks] = useState([]);
  const [editingTask, setEditingTask] = useState(null);

  const addTask = (task) => {
    setTasks([...tasks, task]);
  };

  const editTask = (task) => {
    setTasks(tasks.map(t => t.id === task.id ? task : t));
    setEditingTask(null);
  };

  const deleteTask = (id) => {
    setTasks(tasks.filter(task => task.id !== id));
  };

  return (
    <div>
      <TaskForm onSubmit={editingTask ? editTask : addTask} initialData={editingTask} />
      <TaskList tasks={tasks} onEdit={setEditingTask} onDelete={deleteTask} />
    </div>
  );
};

export default Tasks;
import { useState } from 'react';

const UserList = ({ users, onEdit, onDelete }) => {
  const [filter, setFilter] = useState('');

  const filteredUsers = users.filter(user => 
    user.name.toLowerCase().includes(filter.toLowerCase())
  );

  return (
    <div>
      <h2>User List</h2>
      <input
        type="text"
        value={filter}
        onChange={(e) => setFilter(e.target.value)}
        placeholder="Filter users"
      />
      <ul>
        {filteredUsers.map(user => (
          <li key={user.id}>
            {user.name} 
            <button onClick={() => onEdit(user)}>Edit</button>
            <button onClick={() => onDelete(user.id)}>Delete</button>
          </li>
        ))}
      </ul>
    </div>
  );
};

export default UserList;

import { useState } from 'react';

const TaskList = ({ tasks, onEdit, onDelete }) => {
  const [filter, setFilter] = useState('');

  const filteredTasks = tasks.filter(task =>
    task.title.toLowerCase().includes(filter.toLowerCase())
  );

  return (
    <div>
      <h2>Task List</h2>
      <input
        type="text"
        value={filter}
        onChange={(e) => setFilter(e.target.value)}
        placeholder="Filter tasks"
      />
      <ul>
        {filteredTasks.map(task => (
          <li key={task.id}>
            {task.title}
            <button onClick={() => onEdit(task)}>Edit</button>
            <button onClick={() => onDelete(task.id)}>Delete</button>
          </li>
        ))}
      </ul>
    </div>
  );
};

export default TaskList;
import { useState } from 'react';

const TaskForm = ({ onSubmit, initialData }) => {
  const [title, setTitle] = useState(initialData ? initialData.title : '');

  const handleSubmit = (e) => {
    e.preventDefault();
    onSubmit({ id: initialData ? initialData.id : Date.now(), title });
    setTitle('');
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        value={title}
        onChange={(e) => setTitle(e.target.value)}
        placeholder="Task Title"
        required
      />
      <button type="submit">{initialData ? 'Update' : 'Add'} Task</button>
    </form>
  );
};

export default TaskForm;
import { useState } from 'react';
import TaskList from '../components/TaskList';
import TaskForm from '../components/TaskForm';

const Tasks = () => {
  const [tasks, setTasks] = useState([]);
  const [editingTask, setEditingTask] = useState(null);

  const addTask = (task) => {
    setTasks([...tasks, task]);
  };

  const editTask = (task) => {
    setTasks(tasks.map(t => t.id === task.id ? task : t));
   
npm install
npm run dev
http://localhost:3000
