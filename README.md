import curses

def save_file(content, filename="output.txt"):
    with open(filename, "w") as f:
        f.writelines(content)

def text_editor(stdscr):
    curses.curs_set(1)
    stdscr.clear()
    stdscr.refresh()
    lines = []
    current_line = ""
    row, col = 0, 0

    while True:
        stdscr.clear()
        for i, line in enumerate(lines):
            stdscr.addstr(i, 0, line)
        stdscr.addstr(row, 0, current_line)
        stdscr.move(row, col)
        stdscr.refresh()

        ch = stdscr.getch()

        if ch == curses.KEY_BACKSPACE or ch == 127:
            current_line = current_line[:-1]
            col = max(col - 1, 0)
        elif ch == 10:
            lines.append(current_line)
            current_line = ""
            row += 1
            col = 0
        elif ch == 27:  # ESC to exit and save
            lines.append(current_line)
            save_file(lines)
            break
        else:
            current_line += chr(ch)
            col += 1

curses.wrapper(text_editor)

       
