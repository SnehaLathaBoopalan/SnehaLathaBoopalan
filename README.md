import tkinter as tk
import math

class CircularTimer:
    def __init__(self):
        self.root = tk.Tk()
        self.root.title("Set Your Timer")
        self.root.geometry("300x200")
        self.root.resizable(False, False)

        # Entry widgets for minutes and seconds
        tk.Label(self.root, text="Minutes:").pack(pady=5)
        self.min_entry = tk.Entry(self.root)
        self.min_entry.pack()

        tk.Label(self.root, text="Seconds:").pack(pady=5)
        self.sec_entry = tk.Entry(self.root)
        self.sec_entry.pack()

        # Start button
        tk.Button(self.root, text="Start Timer", command=self.start_timer).pack(pady=10)

        self.root.mainloop()

    def start_timer(self):
        try:
            minutes = int(self.min_entry.get())
            seconds = int(self.sec_entry.get())
            self.duration = minutes * 60 + seconds
            if self.duration <= 0:
                raise ValueError
        except ValueError:
            self.min_entry.delete(0, tk.END)
            self.sec_entry.delete(0, tk.END)
            self.min_entry.insert(0, "0")
            self.sec_entry.insert(0, "0")
            return

        self.root.destroy()
        self.show_timer_window()

    def show_timer_window(self):
        self.remaining = self.duration
        self.timer_root = tk.Tk()
        self.timer_root.title("Circular Countdown Timer")
        self.timer_root.geometry("320x320")
        self.timer_root.resizable(False, False)

        self.canvas = tk.Canvas(self.timer_root, width=300, height=300, bg="white")
        self.canvas.pack(pady=10)

        self.update_timer()
        self.timer_root.mainloop()

    def draw_circle(self):
        self.canvas.delete("all")

        # Grey background circle
        self.canvas.create_oval(50, 50, 250, 250, outline="lightgrey", width=20)

        # Green decreasing arc
        if self.remaining > 0:
            angle = (self.remaining / self.duration) * 360
            self.canvas.create_arc(50, 50, 250, 250, start=90, extent=-angle, outline="green", style='arc', width=20)

        # Display remaining time in MM:SS
        minutes = self.remaining // 60
        seconds = self.remaining % 60
        time_str = f"{minutes:02d}:{seconds:02d}"
        self.canvas.create_text(150, 150, text=time_str, font=("Helvetica", 24, "bold"), fill="black")

    def update_timer(self):
        self.draw_circle()

        if self.remaining > 0:
            self.remaining -= 1
            self.timer_root.after(1000, self.update_timer)
        else:
            self.canvas.create_text(150, 180, text="Time's up!", font=("Helvetica", 16), fill="red")

# 🔄 Start the timer app
CircularTimer()
