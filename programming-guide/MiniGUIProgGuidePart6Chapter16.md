# 34 Spin Box Control

The spin box control described in this chapter makes the user be able to select from a group of predefined values. The interface of a spin box control contains up and down arrows, and the user can select a value by clicking the arrows.

You can create a spin box control by calling CreateWindow function with CTRL_SPINBOX as the control class name.

It should be noted that the size of a spin box control is fixed, that is to say, the width and height of the window passed through CreateWindow function will be ignored.

## 34.1 Styles of Spin Box

Generally, a spin box control has one style SPS_AUTOSCROLL. Spin boxes with this style can automatically determine the present scrolling state of the spin box, and disable (gray) the up and down arrows when it reaches the maximum or the minimum value. The scrolling states of the spin box without this style should be mastered by the application.

## 34.2 Messages of Spin Box
### 34.2.1 Setting/Getting Position

Generally, after a spin box has been created, we can set the attributes and states of the control by sending SPM_SETINFO message to it. Certainly, we can also use this message to reset the attributes of the control:

```cplusplus
SPININFO spinfo;
SendMessage (hwndSpinBox, SPM_SETINFO, 0, (LPARAM)&spinfo) ;
```

In above codes, spinfo is a structure of SPININFO type, and its definition is as following:

```cplusplus
typedef struct _SPININFO
{
    /* The maximum position value */
    int max;
    /* The minimum position value */
    int min;
    /* The current position value */
    int cur;
} SPININFO;
typedef SPININFO *PSPININFO;
```

The fields of SPININFO structure specify the maximum position value, the minimum position value, and the current position value of a spin box control. For the spin box control with SPS_AUTOSCROLL, the following condition must be satisfied: maximum position value >= current position value >= minimum position value.

SPM_GETINFO message is used to get the attributes of a spin box control:

```cplusplus
SPININFO spinfo;
SendMessage (hwndSpinBox, SPM_GETINFO, 0, (LPARAM)&spinfo) ;
```

Here spinfo is used to store the gotten attribute values.

SPM_SETCUR message is used to set the current position of a spin box control:

```cplusplus
int cur;
SendMessage (hwndSpinBox, SPM_SETCUR, cur, 0) ;
```

Here cur is the desired current position of a spin box to be set, and cur should be in the range of the minimum and the maximum when its style is SPS_AUTOSCROLL, otherwise the set will fail, and SendMessage returns -1.

SPM_GETCUR message is used to get the current position:

```cplusplus
int cur;
cur = SendMessage (hwndSpinBox, SPM_GETCUR, 0, 0) ;
```

### 34.2.2 Disabling and Enabling

SPM_DISABLEDOWN, SPM_ENABLEDOWN, SPM_DISABLEUP and SPM_ENABLEUP are used to disable and enable the scrolling ability of the up/down arrow respectively but never mind the current position reached the maximum or minimum. These messages only work for spin boxes without SPS_AUTOSCROLL style. For spin boxes with SPS_AUTOSCROLL style, the scrolling ability and states of the arrows are controlled by the control itself:

```cplusplus
SendMessage (hwndSpinBox, SPM_DISABLEDOWN, 0, 0) ;
SendMessage (hwndSpinBox, SPM_ENABLEDOWN, 0, 0) ;
SendMessage (hwndSpinBox, SPM_DISABLEUP, 0, 0) ;
SendMessage (hwndSpinBox, SPM_ENABLEUP, 0, 0) ;
```

### 34.2.3 Target Window

SPM_SETTARGET message sets the target window of a spin box:

```cplusplus
HWND hTarget;
SendMessage (hwndSpinBox, SPM_SETTARGET, 0, (LPARAM)hTarget) ;
```

When the user clicks the up/down arrow of the spin box, the spin box will send MSG_KEYDOWN and MSG_KEYUP message to the target window. wParam parameter is SCANCODE_CURSORBLOCKUP (when clicking the up arrow) or SCANCODE_CURSORBLOCKDOWN (when clicking the down arrow), and lParam parameter will have the flag of KS_SPINPOST to indicate that the message comes from a spin box.

SPM_GETTARGET message gets the target window of a spin box:

```cplusplus
HWND hTarget;
hTarget = SendMessage (hwndSpinBox, SPM_SETTARGET, 0, 0) ;
```

## 34.3 Notification Codes of Spin Box

A spin box will generate a SPN_REACHMAX notification code when it greater than or equal to the maximum. It will generate a SPN_REACHMIN notification code when it less than or equal to the minimum. 

## 34.4 Sample Program

The program in List 34.1 illustrates the use of a spin box control. Please refer to spinbox.c file of the demo program package of this guide for the complete source code.

<center>List 34.1 Example of spin box</center>

```cplusplus
#define IDC_SPIN        10
#define IDC_CTRL1       20
#define IDC_CTRL2       30
#define IDC_CTRL3       40
#define IDC_CTRL4       50

static int
SpinProc (HWND hDlg, int message, WPARAM wParam, LPARAM lParam)
{
    SPININFO spinfo;
    HWND hSpin = GetDlgItem (hDlg, IDC_SPIN);

    switch (message) {
    case MSG_INITDIALOG:
    {
        /* Set the range and current position of a spin box */
        spinfo.min = 1;
        spinfo.max = 10;
        spinfo.cur = 1;
        SendMessage (hSpin, SPM_SETTARGET, 0, (LPARAM)hDlg);
        SendMessage (hSpin, SPM_SETINFO, 0, (LPARAM)&spinfo);
    }
    break;

    case MSG_KEYDOWN:
    {
        /* Handle the key-pressed messagees, 
         * including the emulate key stroke messages from the spin box
         */
        if (wParam == SCANCODE_CURSORBLOCKUP || 
                  wParam == SCANCODE_CURSORBLOCKDOWN) {
            if (!(lParam & KS_SPINPOST)) {
                int cur;
                cur = SendMessage (hSpin, SPM_GETCUR, 0, 0);
                if (wParam == SCANCODE_CURSORBLOCKUP)
                    cur --;
                else
                    cur ++;
                SendMessage (hSpin, SPM_SETCUR, cur, 0);
            }
            /* Invaldate the window */
            InvalidateRect (hDlg, NULL, TRUE);
        }
    }
    break;

    case MSG_PAINT:
    {
        HDC hdc;
        int x, y, w, h;
        int cur;

        cur = SendMessage (hSpin, SPM_GETCUR, 0, (LPARAM)&spinfo);
        x = 10;
        y = cur*10;
        w = 60;
        h = 10;
        if (y < 10)
            y = 10;
        else if (y > 100)
            y = 100;

        /* Draw the window to reflect the current position of the spin box */
        hdc = BeginPaint (hDlg);
        MoveTo (hdc, 2, 10);
        LineTo (hdc, 100, 10);
        Rectangle (hdc, x, y, x+w, y+h);
        SetBrushColor (hdc, PIXEL_black);
        FillBox (hdc, x, y, w, h);
        MoveTo (hdc, 2, 110);
        LineTo (hdc, 100, 110);
        EndPaint (hDlg, hdc);
    }
    break;

    case MSG_CLOSE:
    {
        EndDialog (hDlg, 0);
    }
    break;

    }

    return DefaultDialogProc (hDlg, message, wParam, lParam);
}

/* The dialog box template */
static DLGTEMPLATE DlgSpin =
{
    WS_BORDER | WS_CAPTION,
    WS_EX_NONE,
    100, 100, 320, 240,
    "spinbox and black block",
    0, 0,
    1, NULL,
    0
};

/* The dialog box has only one control: the spin box control */
static CTRLDATA CtrlSpin[] =
{
    {
        CTRL_SPINBOX,
        SPS_AUTOSCROLL | WS_BORDER | WS_CHILD | WS_VISIBLE,
         200, 120, 0, 0,
        IDC_SPIN,
        "",
        0
    }
};
```

<center>
<img src="%ATTACHURLPATH%/34.1.jpeg" alt="34.1.jpeg"  ALIGN="CENTER" /> <br>
Fig. 34.1 The use of spin box control
</center>

The spinbox.c program creates a spin box control with SPS_AUTOSCROLL in a dialog box, and the user can operate the black block to move between the top and bottom lines by clicking the up and down arrows of the spin box.

-- Main.XiaodongLi - 26 Oct 2009

