from tkinter import *
from tkinter import ttk, messagebox
from tkinter import SEL_FIRST, SEL_LAST, TclError
import base64
import random
import time
import webbrowser

class SecureCrypt:
    def _init_(self, root):
        self.root = root
        self.root.title("🔐 CYBERCOM SECURE MESSAGING v2.4.1")
        self.root.state('zoomed')
        self.root.configure(bg='#0a0a0a')
        
        # Security parameters
        self.security_level = "TOP SECRET//SCI//NOFORN"
        self.user_clearance = "LEVEL 5"
        self.session_id = f"SESSION-{random.randint(100000,999999)}"
        self.encryption_algorithms = ["AES-256-GCM", "ChaCha20-Poly1305", "Twofish-256", "Quantum-Resistant Lattice"]
        
        # Setup UI
        self.setup_ui()
        
        # Security protocols
        self.last_activity = time.time()
        self.root.bind("<Motion>", self.reset_inactivity_timer)
        self.root.bind("<Key>", self.reset_inactivity_timer)
        self.check_inactivity()
        
        # Start system checks
        self.system_check()

    def setup_ui(self):
        """Setup the high-security military interface"""
        # Main container
        self.main_frame = ttk.Frame(self.root)
        self.main_frame.pack(fill=BOTH, expand=True)
        
        # Create tactical grid background
        self.create_tactical_grid()
        
        # Header with system status
        self.create_header()
        
        # Left panel - security controls
        self.create_security_panel()
        
        # Center panel - message operations
        self.create_message_panel()
        
        # Right panel - comms and monitoring
        self.create_comms_panel()
        
        # Status bar
        self.create_status_bar()

    def create_tactical_grid(self):
        """Create tactical grid pattern"""
        self.canvas = Canvas(self.main_frame, bg='#0a0a0a', highlightthickness=0)
        self.canvas.pack(fill=BOTH, expand=True)
        
        width = self.root.winfo_screenwidth()
        height = self.root.winfo_screenheight()
        
        # Draw grid lines
        for x in range(0, width, 25):
            self.canvas.create_line(x, 0, x, height, fill='#0f0f1a', width=1)
        
        for y in range(0, height, 25):
            self.canvas.create_line(0, y, width, y, fill='#0f0f1a', width=1)
        
        # Add crosshair
        center_x, center_y = width//2, height//2
        self.canvas.create_line(center_x-10, center_y, center_x+10, center_y, fill='red', width=2)
        self.canvas.create_line(center_x, center_y-10, center_x, center_y+10, fill='red', width=2)

    def create_header(self):
        """Create the system header with status indicators"""
        header_frame = ttk.Frame(self.main_frame, padding=(10, 5))
        header_frame.pack(fill=X)
        
        # System title
        ttk.Label(header_frame, 
                 text="CYBERCOM SECURE MESSAGING SYSTEM", 
                 font=('Courier', 18, 'bold'),
                 foreground='#00ff00').pack(side=LEFT)
        
        # System status indicators
        status_frame = ttk.Frame(header_frame)
        status_frame.pack(side=RIGHT)
        
        indicators = [
            ("CONNECTION", "SECURE", "#00ff00"),
            ("ENCRYPTION", "ACTIVE", "#00ff00"),
            ("THREAT LEVEL", "GREEN", "#00ff00"),
            ("USER", f"{self.user_clearance}", "#ffff00")
        ]
        
        for text, status, color in indicators:
            frame = ttk.Frame(status_frame)
            frame.pack(side=LEFT, padx=10)
            
            ttk.Label(frame, text=text+":", font=('Courier', 9)).pack(side=LEFT)
            ttk.Label(frame, text=status, font=('Courier', 9, 'bold'), 
                     foreground=color).pack(side=LEFT)

    def create_security_panel(self):
        """Create the security control panel"""
        security_frame = ttk.LabelFrame(self.main_frame, 
                                      text=" SECURITY CONTROLS ",
                                      padding=(15, 10))
        security_frame.place(x=20, y=50, width=300, height=400)
        
        # Authentication
        ttk.Label(security_frame, text="AUTHENTICATION", font=('Courier', 10, 'bold')).pack(anchor=W, pady=(0, 5))
        
        ttk.Label(security_frame, text="OPERATOR CODE:").pack(anchor=W)
        self.password = StringVar()
        ttk.Entry(security_frame, textvariable=self.password, show="•").pack(fill=X)
        
        # Biometric auth
        self.biometric = BooleanVar(value=True)
        ttk.Checkbutton(security_frame, text="BIOMETRIC VERIFICATION", 
                       variable=self.biometric).pack(anchor=W, pady=5)
        
        # Encryption settings
        ttk.Label(security_frame, text="ENCRYPTION PROTOCOL", font=('Courier', 10, 'bold')).pack(anchor=W, pady=(10, 5))
        
        self.encryption_level = StringVar(value=self.encryption_algorithms[0])
        for algo in self.encryption_algorithms:
            ttk.Radiobutton(security_frame, text=algo, 
                           variable=self.encryption_level, 
                           value=algo).pack(anchor=W)
        
        # Self-destruct
        ttk.Label(security_frame, text="AUTO-DESTRUCT TIMER", font=('Courier', 10, 'bold')).pack(anchor=W, pady=(10, 5))
        
        self.destruct_time = IntVar(value=60)
        ttk.Scale(security_frame, from_=10, to=300, variable=self.destruct_time,
                 orient=HORIZONTAL).pack(fill=X)
        ttk.Label(security_frame, textvariable=self.destruct_time).pack()
        
        # Secure actions
        action_frame = ttk.Frame(security_frame)
        action_frame.pack(fill=X, pady=(15, 0))
        
        ttk.Button(action_frame, text="SYSTEM LOCK", 
                  command=self.lock_application).pack(side=LEFT, fill=X, expand=True, padx=2)
        
        ttk.Button(action_frame, text="FULL WIPE", 
                  command=self.emergency_wipe).pack(side=LEFT, fill=X, expand=True, padx=2)

    def create_message_panel(self):
        """Create the main message operation panel"""
        message_frame = ttk.LabelFrame(self.main_frame, 
                                     text=" MESSAGE OPERATIONS ",
                                     padding=(15, 10))
        message_frame.place(x=340, y=50, width=600, height=500)
        
        # Message input
        ttk.Label(message_frame, text="CLASSIFIED MESSAGE INPUT:", font=('Courier', 10, 'bold')).pack(anchor=W)
        
        self.text_input = Text(message_frame, height=15, wrap=WORD, bd=2, 
                             relief=SOLID, bg='#111122', fg='white',
                             font=('Consolas', 12), padx=10, pady=10,
                             insertbackground='white')
        self.text_input.pack(fill=BOTH, expand=True, pady=(0, 10))
        
        # Add scrollbar
        scrollbar = ttk.Scrollbar(message_frame, orient=VERTICAL, command=self.text_input.yview)
        scrollbar.pack(side=RIGHT, fill=Y)
        self.text_input.config(yscrollcommand=scrollbar.set)
        
        # Context menu
        self.create_context_menu()
        self.text_input.bind("<Button-3>", self.show_context_menu)
        
        # Message actions
        action_frame = ttk.Frame(message_frame)
        action_frame.pack(fill=X, pady=(5, 0))
        
        actions = [
            ("ENCRYPT", self.encrypt),
            ("DECRYPT", self.decrypt),
            ("TRANSMIT", self.transmit_message),
            ("SANITIZE", self.reset)
        ]
        
        for text, command in actions:
            ttk.Button(action_frame, text=text, command=command).pack(side=LEFT, fill=X, expand=True, padx=2)

    def create_comms_panel(self):
        """Create communications monitoring panel"""
        comms_frame = ttk.LabelFrame(self.main_frame, 
                                   text=" COMMUNICATIONS MONITOR ",
                                   padding=(15, 10))
        comms_frame.place(x=960, y=50, width=300, height=400)
        
        # Connection status
        ttk.Label(comms_frame, text="CHANNEL STATUS:", font=('Courier', 10, 'bold')).pack(anchor=W)
        
        self.connection_status = ttk.Label(comms_frame, text="SECURE QUANTUM LINK ESTABLISHED",
                                         foreground='#00ff00', font=('Courier', 9))
        self.connection_status.pack(anchor=W, pady=(0, 15))
        
        # Transmission log
        ttk.Label(comms_frame, text="TRANSMISSION LOG:", font=('Courier', 10, 'bold')).pack(anchor=W)
        
        self.transmission_log = Text(comms_frame, height=10, wrap=WORD, bd=2,
                                   relief=SOLID, bg='#111122', fg='#00ff00',
                                   font=('Consolas', 9), padx=10, pady=10)
        self.transmission_log.pack(fill=BOTH, expand=True)
        self.transmission_log.insert(END, "> Secure channel initialized\n")
        self.transmission_log.insert(END, "> Quantum encryption handshake complete\n")
        self.transmission_log.insert(END, f"> Session ID: {self.session_id}\n")
        self.transmission_log.config(state=DISABLED)
        
        # Quick actions
        ttk.Label(comms_frame, text="QUICK ACTIONS:", font=('Courier', 10, 'bold')).pack(anchor=W, pady=(10, 5))
        
        quick_frame = ttk.Frame(comms_frame)
        quick_frame.pack(fill=X)
        
        ttk.Button(quick_frame, text="SEND SITREP", 
                  command=lambda: self.quick_action("SITREP")).pack(side=LEFT, fill=X, expand=True, padx=2)
        
        ttk.Button(quick_frame, text="REQUEST EYES", 
                  command=lambda: self.quick_action("SURVEILLANCE")).pack(side=LEFT, fill=X, expand=True, padx=2)

    def create_status_bar(self):
        """Create the system status bar"""
        status_frame = ttk.Frame(self.main_frame, relief=SUNKEN)
        status_frame.place(x=0, y=460, relwidth=1, height=30)
        
        # Left status
        self.system_status = ttk.Label(status_frame, 
                                     text=f"SYSTEM STATUS: ONLINE | CLEARANCE: {self.user_clearance} | {self.security_level}",
                                     font=('Courier', 9),
                                     foreground='#00ff00')
        self.system_status.pack(side=LEFT, padx=10)
        
        # Right status
        self.time_label = ttk.Label(status_frame, 
                                  font=('Courier', 9))
        self.time_label.pack(side=RIGHT, padx=10)
        self.update_time()
        
        # Center status
        self.operation_status = ttk.Label(status_frame, 
                                        text="READY FOR OPERATION",
                                        font=('Courier', 9))
        self.operation_status.pack(side=RIGHT, padx=10)

    def create_context_menu(self):
        """Create right-click context menu"""
        self.context_menu = Menu(self.root, tearoff=0, 
                               bg='#1a1a3a', fg='white',
                               activebackground='#3a6aaa',
                               activeforeground='white',
                               bd=2, relief=RAISED)
        
        self.context_menu.add_command(label="Cut", command=self.cut_text)
        self.context_menu.add_command(label="Copy", command=self.copy_text)
        self.context_menu.add_command(label="Paste", command=self.paste_text)
        self.context_menu.add_separator()
        self.context_menu.add_command(label="Sanitize Selection", command=self.sanitize_text)
        self.context_menu.add_separator()
        self.context_menu.add_command(label="Classify as TOP SECRET", 
                                    command=lambda: self.classify_text("TOP SECRET"))
        self.context_menu.add_command(label="Classify as SECRET", 
                                    command=lambda: self.classify_text("SECRET"))

    def show_context_menu(self, event):
        """Show the context menu"""
        try:
            self.context_menu.tk_popup(event.x_root, event.y_root)
        finally:
            self.context_menu.grab_release()

    def cut_text(self):
        self.copy_text()
        self.delete_text()

    def copy_text(self):
        try:
            text = self.text_input.get(SEL_FIRST, SEL_LAST)
            self.root.clipboard_clear()
            self.root.clipboard_append(text)
            self.log_activity("Copied text to clipboard - WARNING: Data may be classified")
        except TclError:
            pass

    def paste_text(self):
        try:
            text = self.root.clipboard_get()
            self.text_input.insert(INSERT, text)
        except TclError:
            pass

    def delete_text(self):
        try:
            self.text_input.delete(SEL_FIRST, SEL_LAST)
        except TclError:
            pass

    def sanitize_text(self):
        """Replace selected text with sanitized content"""
        try:
            self.text_input.delete(SEL_FIRST, SEL_LAST)
            self.text_input.insert(INSERT, "[REDACTED]")
            self.log_activity("Text sanitized per security protocol")
        except TclError:
            pass

    def classify_text(self, classification):
        """Add classification markings to text"""
        try:
            text = self.text_input.get(SEL_FIRST, SEL_LAST)
            self.text_input.delete(SEL_FIRST, SEL_LAST)
            self.text_input.insert(INSERT, f"//{classification}// {text} //{classification}//")
            self.log_activity(f"Text marked as {classification}")
        except TclError:
            pass

    def reset_inactivity_timer(self, event=None):
        """Reset the inactivity timer"""
        self.last_activity = time.time()

    def check_inactivity(self):
        """Check for inactivity and lock if needed"""
        if time.time() - self.last_activity > 300:  # 5 minutes
            self.lock_application()
        self.root.after(60000, self.check_inactivity)

    def system_check(self):
        """Perform periodic system checks"""
        # Update system status
        status_messages = [
            "Running cryptographic integrity checks... OK",
            "Verifying quantum link stability... OK",
            "Scanning for anomalies... CLEAR",
            "Monitoring threat channels... NO ACTIVITY"
        ]
        
        for msg in status_messages:
            self.log_activity(msg)
            time.sleep(0.5)
        
        self.root.after(300000, self.system_check)  # Run every 5 minutes

    def update_time(self):
        """Update the time display"""
        current_time = time.strftime("%H:%M:%S UTC | %d-%b-%Y")
        self.time_label.config(text=current_time)
        self.root.after(1000, self.update_time)

    def log_activity(self, message):
        """Log activity to transmission log"""
        timestamp = time.strftime("%H:%M:%S")
        self.transmission_log.config(state=NORMAL)
        self.transmission_log.insert(END, f"> [{timestamp}] {message}\n")
        self.transmission_log.see(END)
        self.transmission_log.config(state=DISABLED)

    def lock_application(self):
        """Lock the application with security screen"""
        self.lock_window = Toplevel(self.root)
        self.lock_window.attributes('-fullscreen', True)
        self.lock_window.configure(bg='black')
        
        # Add security warning
        Label(self.lock_window, 
             text="⚡ SYSTEM LOCKED ⚡", 
             font=('Courier', 36, 'bold'), 
             fg='red', bg='black').pack(pady=50)
        
        Label(self.lock_window, 
             text="UNAUTHORIZED ACCESS PROHIBITED UNDER CYBER SECURITY ACT 2023",
             font=('Courier', 14), 
             fg='white', bg='black').pack(pady=10)
        
        # Security code entry
        Label(self.lock_window, 
             text="ENTER OVERRIDE CODE:", 
             font=('Courier', 14), 
             fg='white', bg='black').pack(pady=10)
        
        self.override_code = StringVar()
        Entry(self.lock_window, 
             textvariable=self.override_code, 
             show="•", 
             font=('Courier', 18), 
             bg='black', fg='white',
             insertbackground='white').pack()
        
        # Unlock button
        Button(self.lock_window, 
              text="AUTHENTICATE", 
              command=self.unlock_application,
              font=('Courier', 14, 'bold'), 
              bg='red', fg='white',
              bd=0).pack(pady=20)
        
        # Add scanning animation
        self.scan_line = Canvas(self.lock_window, bg='black', highlightthickness=0, height=2)
        self.scan_line.pack(fill=X, pady=10)
        self.scan_line.create_line(0, 1, 100, 1, fill='red', width=2, tags='scan')
        self.animate_scan()

    def animate_scan(self):
        """Animate scanning line"""
        width = self.lock_window.winfo_width()
        self.scan_line.delete('scan')
        pos = getattr(self, 'scan_pos', 0)
        
        if pos > width:
            pos = 0
        else:
            pos += 20
        
        self.scan_pos = pos
        self.scan_line.create_line(0, 1, pos, 1, fill='red', width=2, tags='scan')
        self.lock_window.after(50, self.animate_scan)

    def unlock_application(self):
        """Attempt to unlock the application"""
        if self.override_code.get() == "1234":  # Simple override code
            self.lock_window.destroy()
            self.reset_inactivity_timer()
            self.log_activity("System unlocked by operator")
        else:
            self.log_activity("WARNING: Invalid unlock attempt")
            messagebox.showerror("ACCESS DENIED", "Invalid security override code")
            self.override_code.set("")

    def emergency_wipe(self):
        """Emergency wipe all sensitive data"""
        if messagebox.askyesno("CONFIRM EMERGENCY WIPE", 
                             "This will destroy ALL sensitive data!\nProceed?"):
            self.text_input.delete("1.0", END)
            self.password.set("")
            self.transmission_log.config(state=NORMAL)
            self.transmission_log.delete("1.0", END)
            self.transmission_log.insert(END, "> EMERGENCY WIPE COMPLETE\n")
            self.transmission_log.insert(END, "> All data sanitized per protocol 9-5-3\n")
            self.transmission_log.config(state=DISABLED)
            self.log_activity("EMERGENCY WIPE EXECUTED - SYSTEM SANITIZED")

    def quick_action(self, action):
        """Handle quick action buttons"""
        actions = {
            "SITREP": "Situation report transmitted to HQ",
            "SURVEILLANCE": "Surveillance request sent to Recon"
        }
        
        self.log_activity(f"Quick action executed: {action}")
        messagebox.showinfo("ACTION CONFIRMED", actions.get(action, "Action completed"))

    def encrypt(self):
        """Encrypt the current message"""
        if not self.password.get():
            messagebox.showerror("AUTH REQUIRED", "Operator code required")
            return
            
        if self.password.get() != "1234":
            self.log_activity("WARNING: Invalid operator code attempt")
            messagebox.showerror("ACCESS DENIED", "Invalid operator code")
            return
            
        message = self.text_input.get("1.0", END).strip()
        if not message:
            messagebox.showerror("NO INPUT", "No message to encrypt")
            return
            
        try:
            self.operation_status.config(text="ENCRYPTING...", foreground='yellow')
            self.root.update()
            
            # Simulate encryption process
            for i in range(1, 4):
                self.log_activity(f"Encryption pass {i}/3 using {self.encryption_level.get()}")
                time.sleep(0.3)
            
            # Simple base64 encoding for demo
            encoded = base64.b64encode(message.encode()).decode()
            
            # Format encrypted message with metadata
            encrypted = (
                f"//{self.security_level}//\n"
                f"ENCRYPTION: {self.encryption_level.get()}\n"
                f"TIMESTAMP: {time.strftime('%Y-%m-%d %H:%M:%S UTC')}\n"
                f"SESSION: {self.session_id}\n"
                f"AUTO-DESTRUCT: T-{self.destruct_time.get()}s\n"
                f"\n{encoded}\n"
                f"//END ENCRYPTED MESSAGE//"
            )
            
            # Show encrypted message
            self.show_result("ENCRYPTED MESSAGE", encrypted, is_encrypted=True)
            self.log_activity(f"Message encrypted with {self.encryption_level.get()}")
            self.operation_status.config(text="ENCRYPTION SUCCESS", foreground='#00ff00')
            
        except Exception as e:
            self.log_activity(f"ENCRYPTION FAILURE: {str(e)}")
            self.operation_status.config(text="ENCRYPTION FAILED", foreground='red')
            messagebox.showerror("ENCRYPTION ERROR", f"Crypto failure: {str(e)}")

    def decrypt(self):
        """Decrypt the current message - now with CAPTCHA verification"""
        if not self.password.get():
            messagebox.showerror("AUTH REQUIRED", "Operator code required")
            return
            
        if self.password.get() != "1234":
            self.log_activity("WARNING: Invalid operator code attempt")
            messagebox.showerror("ACCESS DENIED", "Invalid operator code")
            return
            
        message = self.text_input.get("1.0", END).strip()
        if not message:
            messagebox.showerror("NO INPUT", "No message to decrypt")
            return
            
        # Show CAPTCHA verification first
        self.show_captcha()

    def show_captcha(self):
        """Show CAPTCHA verification window"""
        self.captcha_window = Toplevel(self.root)
        self.captcha_window.title("CAPTCHA VERIFICATION")
        self.captcha_window.geometry("400x250")
        self.captcha_window.resizable(False, False)
        self.captcha_window.configure(bg='#0a0a0a')
        
        # Generate random CAPTCHA
        self.captcha_code = ''.join(random.choices('ABCDEFGHJKLMNPQRSTUVWXYZ23456789', k=6))
        
        # Create distorted CAPTCHA display
        self.captcha_canvas = Canvas(self.captcha_window, width=300, height=80, bg='#0a0a0a', highlightthickness=0)
        self.captcha_canvas.pack(pady=10)
        
        # Draw CAPTCHA with random distortion
        for i, char in enumerate(self.captcha_code):
            x = 30 + i * 40
            y = 40 + random.randint(-15, 15)
            angle = random.randint(-15, 15)
            self.captcha_canvas.create_text(x, y, text=char, 
                                          font=('Courier', 36, 'bold'),
                                          fill='#00ff00',
                                          angle=angle)
        
        # Add noise lines
        for _ in range(5):
            x1 = random.randint(0, 300)
            y1 = random.randint(0, 80)
            x2 = random.randint(0, 300)
            y2 = random.randint(0, 80)
            self.captcha_canvas.create_line(x1, y1, x2, y2, fill='#00aa00', width=1)
        
        # Entry field
        ttk.Label(self.captcha_window, text="Enter CAPTCHA to verify you're human:", 
                 background='#0a0a0a', foreground='#00ff00').pack(pady=5)
        
        self.captcha_input = ttk.Entry(self.captcha_window, font=('Courier', 14))
        self.captcha_input.pack(pady=5)
        
        # Verify button
        ttk.Button(self.captcha_window, text="VERIFY", 
                  command=self.verify_captcha).pack(pady=10)

    def verify_captcha(self):
        """Verify the CAPTCHA input"""
        if self.captcha_input.get().upper() == self.captcha_code:
            self.captcha_window.destroy()
            self.perform_decryption()
        else:
            messagebox.showerror("CAPTCHA FAILED", "Verification failed. Try again.")
            self.captcha_input.delete(0, END)
            self.log_activity("CAPTCHA verification failed - possible intrusion attempt")

    def perform_decryption(self):
        """Perform the actual decryption after CAPTCHA verification"""
        try:
            message = self.text_input.get("1.0", END).strip()
            self.operation_status.config(text="DECRYPTING...", foreground='yellow')
            self.root.update()
            
            # Extract headers and encoded content
            headers = ""
            encoded = ""
            destruct_time = 60  # Default value
            
            if "\n\n" in message:
                parts = message.split("\n\n")
                headers = parts[0]
                encoded = parts[-1].strip()
                
                # Parse auto-destruct time from headers
                for line in headers.split('\n'):
                    if line.startswith("AUTO-DESTRUCT: T-"):
                        time_str = line.split("T-")[1].replace('s', '')
                        try:
                            destruct_time = int(time_str)
                        except ValueError:
                            destruct_time = 60
                        break
            else:
                encoded = message
                
            # Simple base64 decoding for demo
            decoded = base64.b64decode(encoded.encode()).decode()
            
            # Show decrypted message with self-destruct timer
            self.show_result("DECRYPTED MESSAGE", decoded, 
                           is_encrypted=False, 
                           destruct_time=destruct_time)
            
            self.log_activity("Message decrypted - TOP SECRET CONTENT ACCESSED")
            self.operation_status.config(text="DECRYPTION SUCCESS", foreground='#00ff00')
            
        except Exception as e:
            self.log_activity(f"DECRYPTION FAILURE: {str(e)}")
            self.operation_status.config(text="DECRYPTION FAILED", foreground='red')
            messagebox.showerror("DECRYPTION ERROR", f"Crypto failure: {str(e)}")

    def show_result(self, title, message, is_encrypted, destruct_time=0):
        """Show encrypted/decrypted result with self-destruct timer"""
        window = Toplevel(self.root)
        window.title(title)
        window.geometry("800x600")
        
        # Add security border
        border_color = '#3a4a6a' if is_encrypted else 'red'
        border = Frame(window, bg=border_color, bd=3, relief=RAISED)
        border.place(x=0, y=0, relwidth=1, relheight=1)
        
        content = ttk.Frame(window)
        content.place(relx=0.5, rely=0.5, anchor=CENTER, width=780, height=580)
        
        # Header
        ttk.Label(content, 
                 text=title, 
                 font=('Courier', 14, 'bold')).pack(pady=10)
        
        if not is_encrypted:
            ttk.Label(content, 
                     text="⚠ TOP SECRET CONTENT - EYES ONLY ⚠", 
                     foreground='red',
                     font=('Courier', 12)).pack()
        
        # Message display
        text = Text(content, wrap=WORD, bd=2, 
                   bg='#111122' if is_encrypted else 'black',
                   fg='white' if is_encrypted else '#00ff00',
                   font=('Consolas', 12), padx=10, pady=10)
        text.pack(fill=BOTH, expand=True, padx=10, pady=10)
        text.insert(END, message)
        text.config(state=DISABLED)
        
        # Add scrollbar
        scrollbar = ttk.Scrollbar(content, orient=VERTICAL, command=text.yview)
        scrollbar.pack(side=RIGHT, fill=Y)
        text.config(yscrollcommand=scrollbar.set)
        
        # Add self-destruct timer for decrypted messages
        if not is_encrypted and destruct_time > 0:
            timer_frame = ttk.Frame(content)
            timer_frame.pack(side=BOTTOM, fill=X, pady=5)
            
            # Create a canvas for the digital timer display
            timer_canvas = Canvas(timer_frame, width=300, height=50, bg='black', highlightthickness=0)
            timer_canvas.pack()
            
            # Draw the initial timer display
            timer_text = timer_canvas.create_text(150, 25, 
                                                text=f"♻ {destruct_time:04d} SECONDS TO SELF-DESTRUCT ♻", 
                                                font=('Digital-7', 20, 'bold'),
                                                fill='#00ff00')
            
            def update_timer(count):
                if count >= 0:
                    # Update the timer display
                    timer_canvas.itemconfig(timer_text, text=f"♻ {count:04d} SECONDS TO SELF-DESTRUCT ♻")
                    
                    # Add visual effects for last 5 seconds
                    if count <= 5:
                        flash_color = 'red' if count % 2 == 0 else '#00ff00'
                        timer_canvas.itemconfig(timer_text, fill=flash_color)
                    
                    # Schedule next update
                    window.after(1000, update_timer, count - 1)
                else:
                    # Time's up - destroy the message
                    text.config(state=NORMAL)
                    text.delete("1.0", END)
                    text.insert(END, "🔥 MESSAGE DESTROYED PER SECURITY PROTOCOL 🔥")
                    text.config(state=DISABLED)
                    
                    # Close window after 2 seconds
                    window.after(2000, window.destroy)
                    self.log_activity("Message auto-destruct completed")
            
            # Start the countdown
            update_timer(destruct_time)

        # Action buttons
        btn_frame = ttk.Frame(content)
        btn_frame.pack(fill=X, pady=(0, 10))
        
        ttk.Button(btn_frame, text="COPY", 
                  command=lambda: self.copy_to_clipboard(message)).pack(side=LEFT, fill=X, expand=True, padx=5)
        
        if is_encrypted:
            ttk.Button(btn_frame, text="SEND", 
                      command=lambda: [self.show_transmission_dialog(message), window.destroy()]).pack(side=LEFT, fill=X, expand=True, padx=5)
        else:
            ttk.Button(btn_frame, text="BURN NOW", 
                      command=lambda: [text.config(state=NORMAL),
                                      text.delete("1.0", END),
                                      text.insert(END, "🔥 IMMEDIATE DESTRUCTION INITIATED 🔥"),
                                      text.config(state=DISABLED),
                                      window.after(2000, window.destroy)]).pack(side=LEFT, fill=X, expand=True, padx=5)
        
        ttk.Button(btn_frame, text="CLOSE", 
                  command=window.destroy).pack(side=LEFT, fill=X, expand=True, padx=5)

    def transmit_message(self):
        """Transmit the current message"""
        message = self.text_input.get("1.0", END).strip()
        if not message:
            messagebox.showerror("NO MESSAGE", "No message to transmit")
            return
            
        # Show transmission options
        self.show_transmission_dialog(message)

    def show_transmission_dialog(self, message):
        """Show transmission options dialog"""
        dialog = Toplevel(self.root)
        dialog.title("SELECT TRANSMISSION METHOD")
        dialog.geometry("600x400")
        dialog.resizable(False, False)
        
        # Add security border
        border = Frame(dialog, bg='#3a4a6a', bd=3, relief=RAISED)
        border.place(x=0, y=0, relwidth=1, relheight=1)
        
        content = ttk.Frame(dialog)
        content.place(relx=0.5, rely=0.5, anchor=CENTER, width=580, height=380)
        
        ttk.Label(content, 
                 text="SELECT TRANSMISSION CHANNEL", 
                 font=('Courier', 14, 'bold')).pack(pady=10)
        
        # Transmission options
        options = [
            ("QUANTUM SECURE LINK", "⚛", "End-to-end quantum encrypted"),
            ("SATELLITE UPLINK", "🛰", "Secure military satellite"),
            ("TACTICAL RADIO", "📻", "Encrypted shortwave")
        ]
        
        for name, icon, desc in options:
            frame = ttk.Frame(content, relief=RAISED, borderwidth=1)
            frame.pack(fill=X, padx=20, pady=5)
            
            ttk.Label(frame, text=f"{icon} {name}", font=('Courier', 11)).pack(anchor=W)
            ttk.Label(frame, text=desc, font=('Courier', 8)).pack(anchor=W)
            
            ttk.Button(frame, text="SELECT", 
                      command=lambda n=name: self.send_transmission(n, message, dialog)).pack(side=RIGHT)

    def send_transmission(self, method, message, dialog):
        """Send the transmission via selected method"""
        self.log_activity(f"Initiating {method} transmission")
        dialog.destroy()
        
        # Show progress
        progress = Toplevel(self.root)
        progress.title("TRANSMISSION IN PROGRESS")
        progress.geometry("400x200")
        
        ttk.Label(progress, 
                 text=f"TRANSMITTING VIA {method}", 
                 font=('Courier', 12)).pack(pady=20)
        
        # Progress bar
        pb = ttk.Progressbar(progress, orient=HORIZONTAL, length=300, mode='determinate')
        pb.pack(pady=10)
        
        # Simulate transmission
        for i in range(5):
            pb['value'] = i * 20
            progress.update()
            time.sleep(0.3)
            self.log_activity(f"Transmission {i*20}% complete")
        
        pb['value'] = 100
        progress.destroy()
        
        self.log_activity(f"Transmission via {method} completed successfully")
        messagebox.showinfo("TRANSMISSION COMPLETE", "Message successfully transmitted")

    def burn_message(self, window, text_widget):
        """Burn the message after reading"""
        text_widget.config(state=NORMAL)
        text_widget.delete("1.0", END)
        text_widget.insert(END, "🔥 MESSAGE DESTROYED PER SECURITY PROTOCOL 🔥")
        text_widget.config(state=DISABLED)
        self.log_activity("Message burned after reading")

    def copy_to_clipboard(self, message):
        """Copy message to clipboard"""
        self.root.clipboard_clear()
        self.root.clipboard_append(message)
        self.log_activity("Message copied to clipboard")

    def reset(self):
        """Reset the message area"""
        self.text_input.delete("1.0", END)
        self.password.set("")
        self.log_activity("Message area sanitized")

if _name_ == "_main_":
    root = Tk()
    app = SecureCrypt(root)
    root.mainloop()
