---
layout: page
title: Calendar
permalink: /calendar/
---

# SketchClub Calendar

Stay up to date with all our drawing challenges, workshops, and community events!

<!-- ## 📝 Adding Events

To add new events to the calendar, simply edit the events data in this page. Look for the `events-data` script section and add your event in this format:

```json
"YYYY-MM-DD": {
  "title": "Event Title",
  "type": "challenge|workshop|event",
  "description": "Event description"
}
```

**Event Types:**
- `challenge` - Weekly drawing challenges (green)
- `workshop` - Skill-building workshops (orange) 
- `event` - Community events (blue) -->

<div id="calendar-container">
  <div class="calendar-header">
    <button id="prev-month" class="calendar-nav">&laquo;</button>
    <h2 id="current-month-year"></h2>
    <button id="next-month" class="calendar-nav">&raquo;</button>
  </div>
  
  <div class="calendar-grid">
    <div class="calendar-weekdays">
      <div>Sun</div>
      <div>Mon</div>
      <div>Tue</div>
      <div>Wed</div>
      <div>Thu</div>
      <div>Fri</div>
      <div>Sat</div>
    </div>
    <div id="calendar-days" class="calendar-days"></div>
  </div>
  
  <div class="calendar-events">
    <h3>Upcoming Events</h3>
    <div id="events-list"></div>
  </div>
</div>

<!-- Event Data - Easy to update -->
<script type="application/json" id="events-data">
{
  "2025-10-06": {
    "title": "Sketch Club",
    "type": "event",
    "description": "Sketch Club at 7, Open Mic at 930 in Parish Studios"
  },
  "2025-10-13": {
    "title": "Sketch Club",
    "type": "event",
    "description": "Sketch Club at 7 in Parish Studios"
  },
  "2025-10-20": {
    "title": "Sketch Club",
    "type": "event",
    "description": "Sketch Club at 7, Open Mic at 930 in Parish Studios"
  },
  "2025-10-27": {
   "title": "Sketch Club",
    "type": "event",
    "description": "Sketch Club at 7 in Parish Studios"
  },
  "2025-11-03": {
     "title": "Sketch Club",
    "type": "event",
    "description": "Sketch Club at 7, Open Mic at 930 in Parish Studios"
  }
}
</script>

<script>
// Calendar JavaScript
class SketchClubCalendar {
  constructor() {
    this.currentDate = new Date();
    this.events = {};
    this.loadEvents();
    this.render();
    this.bindEvents();
  }

  loadEvents() {
    const eventsData = document.getElementById('events-data');
    if (eventsData) {
      this.events = JSON.parse(eventsData.textContent);
    }
  }

  render() {
    this.renderCalendar();
    this.renderEvents();
  }

  renderCalendar() {
    const year = this.currentDate.getFullYear();
    const month = this.currentDate.getMonth();
    
    // Update header
    const monthNames = [
      'January', 'February', 'March', 'April', 'May', 'June',
      'July', 'August', 'September', 'October', 'November', 'December'
    ];
    
    document.getElementById('current-month-year').textContent = 
      `${monthNames[month]} ${year}`;

    // Get first day of month and number of days
    const firstDay = new Date(year, month, 1);
    const lastDay = new Date(year, month + 1, 0);
    const daysInMonth = lastDay.getDate();
    const startingDayOfWeek = firstDay.getDay();

    // Clear previous days
    const daysContainer = document.getElementById('calendar-days');
    daysContainer.innerHTML = '';

    // Add empty cells for days before month starts
    for (let i = 0; i < startingDayOfWeek; i++) {
      const emptyDay = document.createElement('div');
      emptyDay.className = 'calendar-day empty';
      daysContainer.appendChild(emptyDay);
    }

    // Add days of the month
    for (let day = 1; day <= daysInMonth; day++) {
      const dayElement = document.createElement('div');
      dayElement.className = 'calendar-day';
      dayElement.textContent = day;
      
      // Check if this day has events
      const dateKey = `${year}-${String(month + 1).padStart(2, '0')}-${String(day).padStart(2, '0')}`;
      if (this.events[dateKey]) {
        dayElement.classList.add('has-event');
        dayElement.title = this.events[dateKey].title;
      }
      
      // Highlight today
      const today = new Date();
      if (year === today.getFullYear() && month === today.getMonth() && day === today.getDate()) {
        dayElement.classList.add('today');
      }
      
      daysContainer.appendChild(dayElement);
    }
  }

  renderEvents() {
    const eventsList = document.getElementById('events-list');
    eventsList.innerHTML = '';
    
    // Get current month's events
    const year = this.currentDate.getFullYear();
    const month = this.currentDate.getMonth();
    
    const monthEvents = Object.keys(this.events)
      .filter(dateKey => {
        const eventDate = new Date(dateKey);
        return eventDate.getFullYear() === year && eventDate.getMonth() === month;
      })
      .sort()
      .slice(0, 5); // Show next 5 events
    
    if (monthEvents.length === 0) {
      eventsList.innerHTML = '<p>No events scheduled for this month.</p>';
      return;
    }
    
    monthEvents.forEach(dateKey => {
      const event = this.events[dateKey];
      const eventDate = new Date(dateKey);
      const eventElement = document.createElement('div');
      eventElement.className = `calendar-event ${event.type}`;
      
      eventElement.innerHTML = `
        <div class="event-date">${eventDate.getDate()}</div>
        <div class="event-details">
          <div class="event-title">${event.title}</div>
          <div class="event-description">${event.description}</div>
        </div>
      `;
      
      eventsList.appendChild(eventElement);
    });
  }

  bindEvents() {
    document.getElementById('prev-month').addEventListener('click', () => {
      this.currentDate.setMonth(this.currentDate.getMonth() - 1);
      this.render();
    });
    
    document.getElementById('next-month').addEventListener('click', () => {
      this.currentDate.setMonth(this.currentDate.getMonth() + 1);
      this.render();
    });
  }
}

// Initialize calendar when page loads
document.addEventListener('DOMContentLoaded', () => {
  new SketchClubCalendar();
});
</script>

<style>
/* Calendar Styles */
#calendar-container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

.calendar-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
  padding: 15px;
  background: #f8f9fa;
  border-radius: 8px;
}

.calendar-nav {
  background: #8B4513;
  color: white;
  border: none;
  padding: 10px 15px;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
}

.calendar-nav:hover {
  background: #D2691E;
}

.calendar-grid {
  background: white;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 10px rgba(0,0,0,0.1);
}

.calendar-weekdays {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  background: #8B4513;
  color: white;
  font-weight: bold;
}

.calendar-weekdays div {
  padding: 15px;
  text-align: center;
}

.calendar-days {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  gap: 1px;
  background: #ddd;
}

.calendar-day {
  background: white;
  padding: 20px;
  text-align: center;
  cursor: pointer;
  transition: background-color 0.2s;
  min-height: 60px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.calendar-day:hover {
  background: #f0f0f0;
}

.calendar-day.today {
  background: #FFF8DC;
  font-weight: bold;
  color: #8B4513;
  border: 2px solid #D2691E;
}

.calendar-day.has-event {
  background: #fff3e0;
  position: relative;
}

.calendar-day.has-event::after {
  content: '';
  position: absolute;
  bottom: 5px;
  left: 50%;
  transform: translateX(-50%);
  width: 6px;
  height: 6px;
  background: #ff9800;
  border-radius: 50%;
}

.calendar-day.empty {
  background: #f5f5f5;
  cursor: default;
}

.calendar-events {
  margin-top: 30px;
  padding: 20px;
  background: #f8f9fa;
  border-radius: 8px;
}

.calendar-events h3 {
  margin-bottom: 20px;
  color: #333;
}

.calendar-event {
  display: flex;
  align-items: center;
  padding: 15px;
  margin-bottom: 10px;
  background: white;
  border-radius: 8px;
  box-shadow: 0 1px 3px rgba(0,0,0,0.1);
}

.calendar-event.challenge {
  border-left: 4px solid #8B4513;
}

.calendar-event.workshop {
  border-left: 4px solid #D2691E;
}

.calendar-event.event {
  border-left: 4px solid #CD853F;
}

.event-date {
  background: #8B4513;
  color: white;
  padding: 10px;
  border-radius: 50%;
  min-width: 40px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: bold;
  margin-right: 15px;
}

.event-details {
  flex: 1;
}

.event-title {
  font-weight: bold;
  color: #333;
  margin-bottom: 5px;
}

.event-description {
  color: #666;
  font-size: 14px;
}

/* Responsive Design */
@media (max-width: 768px) {
  .calendar-day {
    padding: 15px 10px;
    min-height: 50px;
  }
  
  .calendar-event {
    flex-direction: column;
    text-align: center;
  }
  
  .event-date {
    margin-right: 0;
    margin-bottom: 10px;
  }
}
</style>
