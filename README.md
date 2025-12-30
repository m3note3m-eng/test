<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Glass Audio</title>
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'SF Pro Display', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, sans-serif;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            background: linear-gradient(135deg, 
                rgba(20, 20, 60, 0.95) 0%, 
                rgba(10, 10, 40, 0.95) 50%, 
                rgba(5, 5, 30, 0.95) 100%);
            color: rgba(255, 255, 255, 0.95);
            min-height: 100vh;
            padding: 0;
            overflow-x: hidden;
            position: relative;
        }

        body::before {
            content: '';
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: 
                radial-gradient(circle at 20% 50%, rgba(120, 119, 198, 0.15) 0%, transparent 50%),
                radial-gradient(circle at 80% 20%, rgba(255, 119, 230, 0.1) 0%, transparent 50%),
                radial-gradient(circle at 40% 80%, rgba(100, 220, 255, 0.1) 0%, transparent 50%);
            z-index: -1;
            animation: liquidMove 20s ease-in-out infinite alternate;
        }

        @keyframes liquidMove {
            0% {
                transform: translate(0, 0) scale(1);
            }
            100% {
                transform: translate(-50px, -30px) scale(1.1);
            }
        }

        .container {
            max-width: 100%;
            margin: 0 auto;
            padding: 20px;
            backdrop-filter: blur(20px);
            min-height: 100vh;
        }

        .now-playing-card {
            background: linear-gradient(135deg, 
                rgba(255, 255, 255, 0.12) 0%, 
                rgba(255, 255, 255, 0.08) 100%);
            border-radius: 28px;
            padding: 28px;
            margin-bottom: 28px;
            box-shadow: 
                0 8px 32px rgba(0, 0, 0, 0.2),
                inset 0 1px 0 rgba(255, 255, 255, 0.1),
                inset 0 -1px 0 rgba(0, 0, 0, 0.1);
            text-align: center;
            position: relative;
            overflow: hidden;
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .now-playing-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 1px;
            background: linear-gradient(90deg, 
                transparent, 
                rgba(255, 255, 255, 0.3), 
                transparent);
        }

        .track-info {
            margin-bottom: 24px;
            padding: 12px;
            position: relative;
        }

        .track-title {
            font-size: 1.8rem;
            font-weight: 600;
            color: rgba(255, 255, 255, 0.98);
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
            padding: 0 12px;
            margin-bottom: 16px;
            text-shadow: 0 2px 10px rgba(0, 0, 0, 0.3);
            letter-spacing: 0.5px;
        }

        .progress-area {
            margin: 28px 0;
            position: relative;
        }

        .progress-bar {
            height: 6px;
            width: 100%;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 3px;
            cursor: pointer;
            margin-bottom: 10px;
            position: relative;
            overflow: hidden;
            box-shadow: inset 0 1px 2px rgba(0, 0, 0, 0.2);
        }

        .progress-bar::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: linear-gradient(90deg, 
                transparent, 
                rgba(255, 255, 255, 0.05), 
                transparent);
            animation: shimmer 2s infinite linear;
        }

        @keyframes shimmer {
            0% { transform: translateX(-100%); }
            100% { transform: translateX(100%); }
        }

        .progress {
            height: 100%;
            width: 0%;
            background: linear-gradient(90deg, 
                rgba(120, 119, 198, 0.9), 
                rgba(255, 119, 230, 0.9));
            border-radius: 3px;
            transition: width 0.1s linear;
            position: relative;
            box-shadow: 
                0 0 20px rgba(120, 119, 198, 0.3),
                0 0 40px rgba(255, 119, 230, 0.2);
        }

        .progress::after {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: linear-gradient(90deg, 
                transparent, 
                rgba(255, 255, 255, 0.2), 
                transparent);
            animation: progressShimmer 1.5s infinite linear;
        }

        @keyframes progressShimmer {
            0% { transform: translateX(-100%); }
            100% { transform: translateX(100%); }
        }

        .time-info {
            display: flex;
            justify-content: space-between;
            font-size: 0.85rem;
            color: rgba(255, 255, 255, 0.7);
            font-weight: 500;
            letter-spacing: 0.3px;
        }

        .controls {
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 20px;
            margin: 28px 0;
        }

        .control-btn {
            background: linear-gradient(135deg, 
                rgba(255, 255, 255, 0.15) 0%, 
                rgba(255, 255, 255, 0.05) 100%);
            border: 1px solid rgba(255, 255, 255, 0.1);
            width: 56px;
            height: 56px;
            border-radius: 50%;
            color: rgba(255, 255, 255, 0.95);
            font-size: 1.3rem;
            cursor: pointer;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            display: flex;
            align-items: center;
            justify-content: center;
            backdrop-filter: blur(10px);
            box-shadow: 
                0 4px 20px rgba(0, 0, 0, 0.2),
                inset 0 1px 0 rgba(255, 255, 255, 0.1);
        }

        .control-btn:active {
            transform: scale(0.92);
            background: linear-gradient(135deg, 
                rgba(255, 255, 255, 0.2) 0%, 
                rgba(255, 255, 255, 0.1) 100%);
            box-shadow: 
                0 2px 10px rgba(0, 0, 0, 0.2),
                inset 0 1px 0 rgba(255, 255, 255, 0.05);
        }

        .control-btn:hover {
            transform: translateY(-2px);
            box-shadow: 
                0 6px 25px rgba(0, 0, 0, 0.3),
                inset 0 1px 0 rgba(255, 255, 255, 0.15);
        }

        .play-btn {
            width: 72px;
            height: 72px;
            font-size: 1.8rem;
            background: linear-gradient(135deg, 
                rgba(120, 119, 198, 0.9) 0%, 
                rgba(255, 119, 230, 0.9) 100%);
            border: 1px solid rgba(255, 255, 255, 0.3);
            box-shadow: 
                0 8px 32px rgba(120, 119, 198, 0.4),
                inset 0 1px 0 rgba(255, 255, 255, 0.2);
        }

        .play-btn:active {
            transform: scale(0.95);
            box-shadow: 
                0 4px 20px rgba(120, 119, 198, 0.3),
                inset 0 1px 0 rgba(255, 255, 255, 0.1);
        }

        .play-btn:hover {
            transform: translateY(-3px) scale(1.05);
            box-shadow: 
                0 12px 40px rgba(120, 119, 198, 0.5),
                inset 0 1px 0 rgba(255, 255, 255, 0.3);
        }

        .playlist-section {
            margin-top: 36px;
        }

        .section-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 24px;
            padding: 0 8px;
        }

        .section-title {
            font-size: 1.5rem;
            font-weight: 700;
            color: rgba(255, 255, 255, 0.98);
            text-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
            letter-spacing: 0.5px;
            position: relative;
            padding-left: 12px;
        }

        .section-title::before {
            content: '';
            position: absolute;
            left: 0;
            top: 50%;
            transform: translateY(-50%);
            width: 4px;
            height: 24px;
            background: linear-gradient(180deg, 
                rgba(120, 119, 198, 0.9), 
                rgba(255, 119, 230, 0.9));
            border-radius: 2px;
        }

        .section-controls {
            display: flex;
            gap: 12px;
        }

        .add-track-btn, .edit-playlist-btn {
            background: linear-gradient(135deg, 
                rgba(120, 119, 198, 0.9) 0%, 
                rgba(255, 119, 230, 0.9) 100%);
            color: rgba(255, 255, 255, 0.98);
            border: 1px solid rgba(255, 255, 255, 0.2);
            padding: 12px 20px;
            border-radius: 16px;
            font-size: 0.95rem;
            font-weight: 600;
            cursor: pointer;
            display: flex;
            align-items: center;
            gap: 8px;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            backdrop-filter: blur(10px);
            box-shadow: 
                0 4px 20px rgba(120, 119, 198, 0.3),
                inset 0 1px 0 rgba(255, 255, 255, 0.2);
            letter-spacing: 0.3px;
        }

        .edit-playlist-btn {
            background: linear-gradient(135deg, 
                rgba(255, 255, 255, 0.15) 0%, 
                rgba(255, 255, 255, 0.05) 100%);
            box-shadow: 
                0 4px 20px rgba(0, 0, 0, 0.2),
                inset 0 1px 0 rgba(255, 255, 255, 0.1);
        }

        .edit-playlist-btn.active {
            background: linear-gradient(135deg, 
                rgba(120, 119, 198, 0.9) 0%, 
                rgba(255, 119, 230, 0.9) 100%);
            box-shadow: 
                0 4px 20px rgba(120, 119, 198, 0.3),
                inset 0 1px 0 rgba(255, 255, 255, 0.2);
        }

        .add-track-btn:active, .edit-playlist-btn:active {
            transform: scale(0.95);
            box-shadow: 
                0 2px 10px rgba(120, 119, 198, 0.2),
                inset 0 1px 0 rgba(255, 255, 255, 0.1);
        }

        .add-track-btn:hover, .edit-playlist-btn:hover {
            transform: translateY(-2px);
            box-shadow: 
                0 6px 25px rgba(120, 119, 198, 0.4),
                inset 0 1px 0 rgba(255, 255, 255, 0.3);
        }

        .playlist {
            list-style: none;
        }

        .playlist-item {
            padding: 20px;
            background: linear-gradient(135deg, 
                rgba(255, 255, 255, 0.08) 0%, 
                rgba(255, 255, 255, 0.04) 100%);
            border-radius: 20px;
            margin-bottom: 12px;
            display: flex;
            align-items: center;
            cursor: pointer;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            border: 1px solid rgba(255, 255, 255, 0.05);
            backdrop-filter: blur(10px);
            box-shadow: 
                0 4px 20px rgba(0, 0, 0, 0.15),
                inset 0 1px 0 rgba(255, 255, 255, 0.05);
            position: relative;
            overflow: hidden;
        }

        .playlist-item::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 1px;
            background: linear-gradient(90deg, 
                transparent, 
                rgba(255, 255, 255, 0.1), 
                transparent);
        }

        .playlist-item:active {
            transform: scale(0.98);
            background: linear-gradient(135deg, 
                rgba(255, 255, 255, 0.12) 0%, 
                rgba(255, 255, 255, 0.08) 100%);
        }

        .playlist-item.active {
            background: linear-gradient(135deg, 
                rgba(120, 119, 198, 0.2) 0%, 
                rgba(255, 119, 230, 0.2) 100%);
            border: 1px solid rgba(120, 119, 198, 0.3);
            box-shadow: 
                0 4px 25px rgba(120, 119, 198, 0.25),
                inset 0 1px 0 rgba(255, 255, 255, 0.1);
        }

        .playlist-item.active::after {
            content: '';
            position: absolute;
            left: 0;
            top: 0;
            bottom: 0;
            width: 4px;
            background: linear-gradient(180deg, 
                rgba(120, 119, 198, 0.9), 
                rgba(255, 119, 230, 0.9));
            border-radius: 0 2px 2px 0;
        }

        .playlist-icon {
            font-size: 1.3rem;
            margin-right: 20px;
            color: rgba(120, 119, 198, 0.9);
            width: 28px;
            text-align: center;
            text-shadow: 0 0 10px rgba(120, 119, 198, 0.5);
        }

        .playlist-info {
            flex: 1;
            min-width: 0;
        }

        .playlist-title {
            font-weight: 500;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
            color: rgba(255, 255, 255, 0.95);
            font-size: 1.05rem;
            letter-spacing: 0.3px;
        }

        .playlist-controls {
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .edit-btn, .delete-btn, .drag-handle {
            background: none;
            border: 1px solid rgba(255, 255, 255, 0.1);
            color: rgba(255, 255, 255, 0.7);
            font-size: 1.1rem;
            cursor: pointer;
            padding: 8px;
            border-radius: 10px;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            backdrop-filter: blur(5px);
            display: flex;
            align-items: center;
            justify-content: center;
            min-width: 40px;
            min-height: 40px;
        }

        .delete-btn {
            color: rgba(255, 107, 107, 0.9);
            border-color: rgba(255, 107, 107, 0.2);
        }

        .edit-btn {
            color: rgba(120, 119, 198, 0.9);
            border-color: rgba(120, 119, 198, 0.2);
        }

        .drag-handle {
            color: rgba(255, 255, 255, 0.5);
            cursor: grab;
        }

        .drag-handle:active {
            cursor: grabbing;
        }

        .edit-btn:hover, .delete-btn:hover, .drag-handle:hover {
            transform: translateY(-2px);
            background: rgba(255, 255, 255, 0.1);
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
        }

        .edit-btn:active, .delete-btn:active, .drag-handle:active {
            transform: scale(0.95);
        }

        .empty-playlist {
            text-align: center;
            padding: 80px 20px;
            color: rgba(255, 255, 255, 0.5);
            background: linear-gradient(135deg, 
                rgba(255, 255, 255, 0.05) 0%, 
                rgba(255, 255, 255, 0.02) 100%);
            border-radius: 20px;
            border: 1px solid rgba(255, 255, 255, 0.05);
            backdrop-filter: blur(10px);
            box-shadow: 
                0 4px 20px rgba(0, 0, 0, 0.1),
                inset 0 1px 0 rgba(255, 255, 255, 0.05);
        }

        .empty-playlist i {
            font-size: 3.5rem;
            margin-bottom: 20px;
            opacity: 0.3;
            text-shadow: 0 0 20px rgba(255, 255, 255, 0.1);
        }

        .empty-playlist p {
            margin-bottom: 12px;
            font-size: 1.1rem;
            letter-spacing: 0.5px;
        }

        .modal {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(0, 0, 0, 0.7);
            backdrop-filter: blur(10px);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 1000;
            padding: 20px;
            opacity: 0;
            visibility: hidden;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }

        .modal.active {
            opacity: 1;
            visibility: visible;
        }

        .modal-content {
            background: linear-gradient(135deg, 
                rgba(255, 255, 255, 0.12) 0%, 
                rgba(255, 255, 255, 0.08) 100%);
            border-radius: 28px;
            padding: 32px;
            width: 100%;
            max-width: 420px;
            max-height: 90vh;
            overflow-y: auto;
            transform: translateY(30px) scale(0.95);
            transition: transform 0.4s cubic-bezier(0.4, 0, 0.2, 1);
            border: 1px solid rgba(255, 255, 255, 0.1);
            box-shadow: 
                0 20px 60px rgba(0, 0, 0, 0.3),
                inset 0 1px 0 rgba(255, 255, 255, 0.1);
            position: relative;
            overflow: hidden;
        }

        .modal-content::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 1px;
            background: linear-gradient(90deg, 
                transparent, 
                rgba(255, 255, 255, 0.3), 
                transparent);
        }

        .modal.active .modal-content {
            transform: translateY(0) scale(1);
        }

        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 28px;
            padding-bottom: 20px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }

        .modal-title {
            font-size: 1.5rem;
            font-weight: 700;
            color: rgba(255, 255, 255, 0.98);
            text-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
            letter-spacing: 0.5px;
        }

        .close-btn {
            background: rgba(255, 255, 255, 0.1);
            border: 1px solid rgba(255, 255, 255, 0.1);
            color: rgba(255, 255, 255, 0.7);
            font-size: 1.3rem;
            cursor: pointer;
            padding: 10px;
            border-radius: 12px;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            display: flex;
            align-items: center;
            justify-content: center;
            width: 44px;
            height: 44px;
        }

        .close-btn:hover {
            background: rgba(255, 255, 255, 0.15);
            transform: rotate(90deg);
        }

        .close-btn:active {
            transform: scale(0.9) rotate(90deg);
        }

        .form-group {
            margin-bottom: 24px;
        }

        .form-label {
            display: block;
            margin-bottom: 12px;
            font-size: 0.95rem;
            color: rgba(255, 255, 255, 0.8);
            font-weight: 500;
            letter-spacing: 0.3px;
        }

        .form-input {
            width: 100%;
            padding: 16px 20px;
            background: rgba(255, 255, 255, 0.07);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 16px;
            color: rgba(255, 255, 255, 0.95);
            font-size: 1rem;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            backdrop-filter: blur(10px);
            box-shadow: 
                inset 0 2px 4px rgba(0, 0, 0, 0.1),
                0 1px 0 rgba(255, 255, 255, 0.05);
        }

        .form-input:focus {
            outline: none;
            border-color: rgba(120, 119, 198, 0.5);
            background: rgba(255, 255, 255, 0.1);
            box-shadow: 
                inset 0 2px 8px rgba(0, 0, 0, 0.2),
                0 0 0 3px rgba(120, 119, 198, 0.1);
        }

        .form-input::placeholder {
            color: rgba(255, 255, 255, 0.4);
        }

        .submit-btn {
            width: 100%;
            padding: 18px;
            background: linear-gradient(135deg, 
                rgba(120, 119, 198, 0.9) 0%, 
                rgba(255, 119, 230, 0.9) 100%);
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-radius: 16px;
            color: rgba(255, 255, 255, 0.98);
            font-size: 1.05rem;
            font-weight: 600;
            cursor: pointer;
            margin-top: 12px;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            letter-spacing: 0.5px;
            text-shadow: 0 1px 2px rgba(0, 0, 0, 0.2);
            box-shadow: 
                0 4px 20px rgba(120, 119, 198, 0.3),
                inset 0 1px 0 rgba(255, 255, 255, 0.2);
        }

        .submit-btn:active {
            transform: scale(0.98);
            box-shadow: 
                0 2px 10px rgba(120, 119, 198, 0.2),
                inset 0 1px 0 rgba(255, 255, 255, 0.1);
        }

        .submit-btn:hover {
            transform: translateY(-2px);
            box-shadow: 
                0 6px 25px rgba(120, 119, 198, 0.4),
                inset 0 1px 0 rgba(255, 255, 255, 0.3);
        }

        .toast {
            position: fixed;
            bottom: 120px;
            left: 50%;
            transform: translateX(-50%) translateY(100px) scale(0.9);
            background: linear-gradient(135deg, 
                rgba(255, 255, 255, 0.15) 0%, 
                rgba(255, 255, 255, 0.08) 100%);
            color: rgba(255, 255, 255, 0.95);
            padding: 18px 28px;
            border-radius: 20px;
            box-shadow: 
                0 15px 35px rgba(0, 0, 0, 0.25),
                inset 0 1px 0 rgba(255, 255, 255, 0.1);
            z-index: 1000;
            opacity: 0;
            transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
            text-align: center;
            max-width: 90%;
            border: 1px solid rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(20px);
            font-weight: 500;
            letter-spacing: 0.3px;
        }

        .toast.show {
            transform: translateX(-50%) translateY(0) scale(1);
            opacity: 1;
        }

        .toast.success {
            border-left: 4px solid rgba(76, 175, 80, 0.8);
            background: linear-gradient(135deg, 
                rgba(76, 175, 80, 0.15) 0%, 
                rgba(76, 175, 80, 0.08) 100%);
        }

        .toast.error {
            border-left: 4px solid rgba(244, 67, 54, 0.8);
            background: linear-gradient(135deg, 
                rgba(244, 67, 54, 0.15) 0%, 
                rgba(244, 67, 54, 0.08) 100%);
        }

        @media (max-width: 480px) {
            .container {
                padding: 16px;
            }
            
            .track-title {
                font-size: 1.6rem;
            }
            
            .controls {
                gap: 16px;
            }
            
            .control-btn {
                width: 52px;
                height: 52px;
                font-size: 1.2rem;
            }
            
            .play-btn {
                width: 68px;
                height: 68px;
                font-size: 1.7rem;
            }
            
            .section-controls {
                gap: 10px;
            }
            
            .add-track-btn, .edit-playlist-btn {
                padding: 10px 16px;
                font-size: 0.9rem;
            }
            
            .modal-content {
                padding: 24px;
            }
        }

        @media (max-width: 360px) {
            .track-title {
                font-size: 1.4rem;
            }
            
            .section-controls {
                flex-direction: column;
                gap: 8px;
            }
        }

        .playlist-item.dragging {
            opacity: 0.5;
            background: linear-gradient(135deg, 
                rgba(120, 119, 198, 0.15) 0%, 
                rgba(255, 119, 230, 0.15) 100%);
            transform: scale(0.95);
        }

        .playlist-item.drag-over {
            border-top: 2px solid rgba(120, 119, 198, 0.9);
            margin-top: 2px;
        }

        /* Анимация появления элементов */
        @keyframes fadeInUp {
            from {
                opacity: 0;
                transform: translateY(20px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .now-playing-card, .playlist-section {
            animation: fadeInUp 0.6s cubic-bezier(0.4, 0, 0.2, 1);
        }

        /* Анимация переливания цвета */
        @keyframes colorFlow {
            0%, 100% {
                filter: hue-rotate(0deg);
            }
            50% {
                filter: hue-rotate(30deg);
            }
        }

        .play-btn, .progress, .add-track-btn, .edit-playlist-btn.active, .submit-btn {
            animation: colorFlow 10s infinite linear;
        }
    </style>
</head>
<body>
    <div class="container">
        <main>
            <div class="now-playing-card">
                <div class="track-info">
                    <h3 class="track-title" id="track-title">Выберите трек</h3>
                </div>

                <div class="progress-area">
                    <div class="progress-bar" id="progress-bar">
                        <div class="progress" id="progress"></div>
                    </div>
                    <div class="time-info">
                        <span id="current-time">0:00</span>
                        <span id="total-time">0:00</span>
                    </div>
                </div>

                <div class="controls">
                    <button class="control-btn" id="prev-btn">
                        <i class="fas fa-step-backward"></i>
                    </button>
                    <button class="control-btn play-btn" id="play-btn">
                        <i class="fas fa-play" id="play-icon"></i>
                    </button>
                    <button class="control-btn" id="next-btn">
                        <i class="fas fa-step-forward"></i>
                    </button>
                </div>
            </div>

            <section class="playlist-section">
                <div class="section-header">
                    <h3 class="section-title">Плейлист</h3>
                    <div class="section-controls">
                        <button class="add-track-btn" id="add-track-btn">
                            <i class="fas fa-plus"></i> Добавить
                        </button>
                        <button class="edit-playlist-btn" id="edit-playlist-btn">
                            <i class="fas fa-edit"></i> Редактировать
                        </button>
                    </div>
                </div>

                <ul class="playlist" id="playlist">
                    <!-- Плейлист будет заполняться динамически -->
                </ul>

                <div class="empty-playlist" id="empty-playlist">
                    <i class="fas fa-music"></i>
                    <p>Плейлист пуст</p>
                    <p>Добавьте первый трек</p>
                </div>
            </section>
        </main>
    </div>

    <!-- Модальное окно добавления трека -->
    <div class="modal" id="add-track-modal">
        <div class="modal-content">
            <div class="modal-header">
                <h3 class="modal-title">Добавить трек</h3>
                <button class="close-btn" id="close-modal-btn">
                    <i class="fas fa-times"></i>
                </button>
            </div>
            
            <div class="form-group">
                <label class="form-label">Ссылка на аудиофайл</label>
                <input type="text" id="track-url" class="form-input" placeholder="https://example.com/audio.mp3">
            </div>
            
            <div class="form-group">
                <label class="form-label">Название трека</label>
                <input type="text" id="track-name" class="form-input" placeholder="Название трека">
            </div>
            
            <button class="submit-btn" id="submit-track-btn">Добавить в плейлист</button>
        </div>
    </div>

    <!-- Модальное окно редактирования трека -->
    <div class="modal" id="edit-track-modal">
        <div class="modal-content">
            <div class="modal-header">
                <h3 class="modal-title">Редактировать трек</h3>
                <button class="close-btn" id="close-edit-modal-btn">
                    <i class="fas fa-times"></i>
                </button>
            </div>
            
            <div class="form-group">
                <label class="form-label">Ссылка на аудиофайл</label>
                <input type="text" id="edit-track-url" class="form-input" placeholder="https://example.com/audio.mp3">
            </div>
            
            <div class="form-group">
                <label class="form-label">Название трека</label>
                <input type="text" id="edit-track-name" class="form-input" placeholder="Название трека">
            </div>
            
            <button class="submit-btn" id="submit-edit-track-btn">Сохранить изменения</button>
        </div>
    </div>

    <!-- Toast сообщения -->
    <div class="toast" id="toast"></div>

    <audio id="audio-player" preload="metadata"></audio>

    <script>
        // Telegram WebApp API
        const tg = window.Telegram?.WebApp;
        
        // Инициализация Telegram WebApp
        if (tg) {
            tg.expand(); // Раскрываем приложение на весь экран
            tg.BackButton.hide(); // Скрываем кнопку "Назад"
            
            // Применяем тему Telegram
            document.documentElement.style.setProperty('--tg-theme-bg-color', tg.themeParams.bg_color || '#1a1a2e');
            document.documentElement.style.setProperty('--tg-theme-text-color', tg.themeParams.text_color || '#ffffff');
            document.documentElement.style.setProperty('--tg-theme-hint-color', tg.themeParams.hint_color || '#a0a0c0');
            document.documentElement.style.setProperty('--tg-theme-button-color', tg.themeParams.button_color || '#2481cc');
            document.documentElement.style.setProperty('--tg-theme-button-text-color', tg.themeParams.button_text_color || '#ffffff');
            document.documentElement.style.setProperty('--tg-theme-secondary-bg-color', tg.themeParams.secondary_bg_color || 'rgba(255, 255, 255, 0.05)');
        }

        // Элементы DOM
        const audioPlayer = document.getElementById('audio-player');
        const playBtn = document.getElementById('play-btn');
        const playIcon = document.getElementById('play-icon');
        const prevBtn = document.getElementById('prev-btn');
        const nextBtn = document.getElementById('next-btn');
        const progressBar = document.getElementById('progress-bar');
        const progress = document.getElementById('progress');
        const currentTimeEl = document.getElementById('current-time');
        const totalTimeEl = document.getElementById('total-time');
        const trackTitle = document.getElementById('track-title');
        const playlistEl = document.getElementById('playlist');
        const emptyPlaylistEl = document.getElementById('empty-playlist');
        const addTrackBtn = document.getElementById('add-track-btn');
        const editPlaylistBtn = document.getElementById('edit-playlist-btn');
        const addTrackModal = document.getElementById('add-track-modal');
        const editTrackModal = document.getElementById('edit-track-modal');
        const closeModalBtn = document.getElementById('close-modal-btn');
        const closeEditModalBtn = document.getElementById('close-edit-modal-btn');
        const submitTrackBtn = document.getElementById('submit-track-btn');
        const submitEditTrackBtn = document.getElementById('submit-edit-track-btn');
        const trackUrlInput = document.getElementById('track-url');
        const trackNameInput = document.getElementById('track-name');
        const editTrackUrlInput = document.getElementById('edit-track-url');
        const editTrackNameInput = document.getElementById('edit-track-name');
        const toast = document.getElementById('toast');

        // Состояние плеера
        let currentTrackIndex = 0;
        let isPlaying = false;
        let playlist = [];
        let isEditMode = false;
        let currentEditIndex = -1;
        let dragStartIndex = -1;

        // Загружаем плейлист из localStorage
        function loadPlaylist() {
            const savedPlaylist = localStorage.getItem('tgAudioPlayerPlaylist');
            if (savedPlaylist) {
                try {
                    playlist = JSON.parse(savedPlaylist);
                    renderPlaylist();
                    if (playlist.length > 0) {
                        loadTrack(currentTrackIndex, false);
                    }
                } catch (e) {
                    console.error('Ошибка загрузки плейлиста:', e);
                    playlist = [];
                }
            }
            updateEmptyPlaylistVisibility();
        }

        // Сохраняем плейлист в localStorage
        function savePlaylist() {
            localStorage.setItem('tgAudioPlayerPlaylist', JSON.stringify(playlist));
        }

        // Отображаем плейлист
        function renderPlaylist() {
            playlistEl.innerHTML = '';
            
            playlist.forEach((track, index) => {
                const li = document.createElement('li');
                li.className = `playlist-item ${index === currentTrackIndex ? 'active' : ''}`;
                li.dataset.index = index;
                li.style.animation = 'fadeInUp 0.4s ease-out';
                li.innerHTML = `
                    <div class="playlist-icon">
                        <i class="fas fa-music"></i>
                    </div>
                    <div class="playlist-info">
                        <div class="playlist-title">${track.name || 'Без названия'}</div>
                    </div>
                    <div class="playlist-controls">
                        ${isEditMode ? `
                            <button class="edit-btn" data-index="${index}">
                                <i class="fas fa-edit"></i>
                            </button>
                            <button class="delete-btn" data-index="${index}">
                                <i class="fas fa-trash"></i>
                            </button>
                            <button class="drag-handle" data-index="${index}">
                                <i class="fas fa-bars"></i>
                            </button>
                        ` : `
                            <button class="delete-btn" data-index="${index}">
                                <i class="fas fa-trash"></i>
                            </button>
                        `}
                    </div>
                `;
                
                // Обработка клика по элементу плейлиста
                li.addEventListener('click', (e) => {
                    if (!e.target.closest('.playlist-controls')) {
                        playTrack(index);
                    }
                });
                
                // Добавляем обработчики для режима редактирования
                if (isEditMode) {
                    const dragHandle = li.querySelector('.drag-handle');
                    const editBtn = li.querySelector('.edit-btn');
                    const deleteBtn = li.querySelector('.delete-btn');
                    
                    // Перетаскивание
                    dragHandle.addEventListener('mousedown', (e) => startDrag(e, index));
                    dragHandle.addEventListener('touchstart', (e) => startDrag(e, index));
                    
                    // Редактирование
                    editBtn.addEventListener('click', (e) => {
                        e.stopPropagation();
                        editTrack(index);
                    });
                    
                    // Удаление
                    deleteBtn.addEventListener('click', (e) => {
                        e.stopPropagation();
                        deleteTrack(index);
                    });
                } else {
                    // Обычный режим - только удаление
                    const deleteBtn = li.querySelector('.delete-btn');
                    deleteBtn.addEventListener('click', (e) => {
                        e.stopPropagation();
                        deleteTrack(index);
                    });
                }
                
                playlistEl.appendChild(li);
            });
            
            updateEmptyPlaylistVisibility();
        }

        // Обновляем видимость сообщения о пустом плейлисте
        function updateEmptyPlaylistVisibility() {
            if (playlist.length === 0) {
                emptyPlaylistEl.style.display = 'block';
            } else {
                emptyPlaylistEl.style.display = 'none';
            }
        }

        // Добавляем трек в плейлист
        function addTrack(url, name) {
            if (!url) {
                showToast('Введите ссылку на аудиофайл', 'error');
                return;
            }
            
            // Проверяем URL
            if (!url.startsWith('http://') && !url.startsWith('https://')) {
                showToast('Неверный формат ссылки', 'error');
                return;
            }
            
            // Проверяем, есть ли уже такой трек
            if (playlist.some(track => track.url === url)) {
                showToast('Этот трек уже есть в плейлисте', 'error');
                return;
            }
            
            const newTrack = {
                url,
                name: name || 'Без названия'
            };
            
            playlist.push(newTrack);
            savePlaylist();
            renderPlaylist();
            
            // Если это первый трек, загружаем его
            if (playlist.length === 1) {
                playTrack(0);
            }
            
            showToast('Трек добавлен в плейлист', 'success');
            closeModal();
            
            // Очищаем поля
            trackUrlInput.value = '';
            trackNameInput.value = '';
        }

        // Редактируем трек
        function editTrack(index) {
            if (index < 0 || index >= playlist.length) return;
            
            const track = playlist[index];
            currentEditIndex = index;
            
            // Заполняем форму текущими значениями
            editTrackUrlInput.value = track.url;
            editTrackNameInput.value = track.name;
            
            // Показываем модальное окно редактирования
            editTrackModal.classList.add('active');
        }

        // Сохраняем изменения трека
        function saveEditedTrack() {
            if (currentEditIndex < 0 || currentEditIndex >= playlist.length) return;
            
            const url = editTrackUrlInput.value.trim();
            const name = editTrackNameInput.value.trim();
            
            if (!url) {
                showToast('Введите ссылку на аудиофайл', 'error');
                return;
            }
            
            // Проверяем URL
            if (!url.startsWith('http://') && !url.startsWith('https://')) {
                showToast('Неверный формат ссылки', 'error');
                return;
            }
            
            // Проверяем, есть ли уже такой трек (кроме текущего)
            if (playlist.some((track, index) => track.url === url && index !== currentEditIndex)) {
                showToast('Этот трек уже есть в плейлисте', 'error');
                return;
            }
            
            // Обновляем трек
            playlist[currentEditIndex] = {
                url,
                name: name || 'Без названия'
            };
            
            savePlaylist();
            renderPlaylist();
            
            // Если редактировали текущий трек, обновляем его
            if (currentEditIndex === currentTrackIndex) {
                loadTrack(currentTrackIndex, false);
            }
            
            showToast('Трек обновлен', 'success');
            closeEditModal();
        }

        // Удаляем трек из плейлиста
        function deleteTrack(index) {
            if (index < 0 || index >= playlist.length) return;
            
            playlist.splice(index, 1);
            
            // Корректируем текущий индекс
            if (currentTrackIndex >= index && currentTrackIndex > 0) {
                currentTrackIndex--;
            }
            
            // Если плейлист пуст
            if (playlist.length === 0) {
                audioPlayer.src = '';
                trackTitle.textContent = 'Выберите трек';
                isPlaying = false;
                playIcon.className = 'fas fa-play';
                progress.style.width = '0%';
                currentTimeEl.textContent = '0:00';
                totalTimeEl.textContent = '0:00';
            } else {
                playTrack(currentTrackIndex);
            }
            
            savePlaylist();
            renderPlaylist();
            showToast('Трек удален', 'success');
        }

        // Переключение режима редактирования
        function toggleEditMode() {
            isEditMode = !isEditMode;
            
            if (isEditMode) {
                editPlaylistBtn.classList.add('active');
                editPlaylistBtn.innerHTML = '<i class="fas fa-check"></i> Готово';
                showToast('Режим редактирования включен', 'info');
            } else {
                editPlaylistBtn.classList.remove('active');
                editPlaylistBtn.innerHTML = '<i class="fas fa-edit"></i> Редактировать';
                showToast('Режим редактирования выключен', 'info');
            }
            
            renderPlaylist();
        }

        // Функции для перетаскивания
        function startDrag(e, index) {
            e.preventDefault();
            dragStartIndex = index;
            
            const li = playlistEl.children[index];
            li.classList.add('dragging');
            
            // Добавляем обработчики для перетаскивания
            document.addEventListener('mousemove', handleDrag);
            document.addEventListener('mouseup', stopDrag);
            document.addEventListener('touchmove', handleDrag);
            document.addEventListener('touchend', stopDrag);
        }

        function handleDrag(e) {
            e.preventDefault();
            
            const clientY = e.clientY || (e.touches && e.touches[0].clientY);
            if (!clientY) return;
            
            // Находим элемент, над которым происходит перетаскивание
            const playlistItems = Array.from(playlistEl.children);
            const draggingItem = playlistEl.children[dragStartIndex];
            
            // Убираем класс drag-over у всех элементов
            playlistItems.forEach(item => item.classList.remove('drag-over'));
            
            // Находим элемент, над которым находимся
            const dragOverItem = playlistItems.find(item => {
                const rect = item.getBoundingClientRect();
                return clientY > rect.top && clientY < rect.bottom && item !== draggingItem;
            });
            
            if (dragOverItem) {
                dragOverItem.classList.add('drag-over');
            }
        }

        function stopDrag(e) {
            e.preventDefault();
            
            const clientY = e.clientY || (e.changedTouches && e.changedTouches[0].clientY);
            if (!clientY || dragStartIndex === -1) return;
            
            // Убираем классы
            const playlistItems = Array.from(playlistEl.children);
            playlistItems.forEach(item => {
                item.classList.remove('dragging');
                item.classList.remove('drag-over');
            });
            
            // Находим конечную позицию
            let dragEndIndex = -1;
            for (let i = 0; i < playlistItems.length; i++) {
                const rect = playlistItems[i].getBoundingClientRect();
                if (clientY > rect.top && clientY < rect.bottom) {
                    dragEndIndex = i;
                    break;
                }
            }
            
            // Если нашли позицию и она отличается от начальной
            if (dragEndIndex !== -1 && dragEndIndex !== dragStartIndex) {
                // Перемещаем трек в плейлисте
                const [draggedTrack] = playlist.splice(dragStartIndex, 1);
                playlist.splice(dragEndIndex, 0, draggedTrack);
                
                // Обновляем текущий индекс, если нужно
                if (currentTrackIndex === dragStartIndex) {
                    currentTrackIndex = dragEndIndex;
                } else if (currentTrackIndex > dragStartIndex && currentTrackIndex <= dragEndIndex) {
                    currentTrackIndex--;
                } else if (currentTrackIndex < dragStartIndex && currentTrackIndex >= dragEndIndex) {
                    currentTrackIndex++;
                }
                
                savePlaylist();
                renderPlaylist();
                showToast('Порядок треков изменен', 'success');
            }
            
            // Сбрасываем состояние
            dragStartIndex = -1;
            
            // Убираем обработчики
            document.removeEventListener('mousemove', handleDrag);
            document.removeEventListener('mouseup', stopDrag);
            document.removeEventListener('touchmove', handleDrag);
            document.removeEventListener('touchend', stopDrag);
        }

        // Загружаем трек
        function loadTrack(index, autoPlay = true) {
            if (index < 0 || index >= playlist.length) return;
            
            const track = playlist[index];
            
            // Обновляем информацию
            trackTitle.textContent = track.name;
            
            // Загружаем аудио
            audioPlayer.src = track.url;
            
            // Устанавливаем фиксированную громкость
            audioPlayer.volume = 1.0;
            
            currentTrackIndex = index;
            renderPlaylist();
            
            // Автовоспроизведение
            if (autoPlay) {
                setTimeout(() => {
                    playAudio();
                }, 100);
            }
        }

        // Воспроизводим трек
        function playTrack(index) {
            if (index < 0 || index >= playlist.length) return;
            loadTrack(index);
        }

        // Управление воспроизведением
        function playAudio() {
            if (playlist.length === 0) return;
            
            if (audioPlayer.paused) {
                audioPlayer.play().then(() => {
                    isPlaying = true;
                    playIcon.className = 'fas fa-pause';
                }).catch(e => {
                    console.error('Ошибка воспроизведения:', e);
                    showToast('Ошибка воспроизведения аудио', 'error');
                });
            } else {
                audioPlayer.pause();
                isPlaying = false;
                playIcon.className = 'fas fa-play';
            }
        }

        // Следующий трек
        function nextTrack() {
            if (playlist.length === 0) return;
            
            let nextIndex = currentTrackIndex + 1;
            if (nextIndex >= playlist.length) {
                nextIndex = 0;
            }
            
            playTrack(nextIndex);
        }

        // Предыдущий трек
        function prevTrack() {
            if (playlist.length === 0) return;
            
            let prevIndex = currentTrackIndex - 1;
            if (prevIndex < 0) {
                prevIndex = playlist.length - 1;
            }
            
            playTrack(prevIndex);
        }

        // Форматирование времени
        function formatTime(seconds) {
            if (isNaN(seconds)) return '0:00';
            
            const mins = Math.floor(seconds / 60);
            const secs = Math.floor(seconds % 60);
            return `${mins}:${secs < 10 ? '0' : ''}${secs}`;
        }

        // Обновление прогресса
        function updateProgress() {
            const { currentTime, duration } = audioPlayer;
            
            if (isNaN(duration)) return;
            
            const progressPercent = (currentTime / duration) * 100;
            progress.style.width = `${progressPercent}%`;
            
            currentTimeEl.textContent = formatTime(currentTime);
            totalTimeEl.textContent = formatTime(duration);
        }

        // Установка прогресса
        function setProgress(e) {
            const width = this.clientWidth;
            const clickX = e.offsetX;
            const duration = audioPlayer.duration;
            
            if (isNaN(duration)) return;
            
            audioPlayer.currentTime = (clickX / width) * duration;
        }

        // Показ toast сообщения
        function showToast(message, type = 'info') {
            toast.textContent = message;
            toast.className = `toast ${type}`;
            toast.classList.add('show');
            
            setTimeout(() => {
                toast.classList.remove('show');
            }, 3000);
        }

        // Открытие модального окна
        function openModal() {
            addTrackModal.classList.add('active');
        }

        // Закрытие модального окна
        function closeModal() {
            addTrackModal.classList.remove('active');
        }

        // Закрытие модального окна редактирования
        function closeEditModal() {
            editTrackModal.classList.remove('active');
            currentEditIndex = -1;
            editTrackUrlInput.value = '';
            editTrackNameInput.value = '';
        }

        // Инициализация
        function init() {
            // Загружаем плейлист
            loadPlaylist();
            
            // Назначаем обработчики
            playBtn.addEventListener('click', playAudio);
            prevBtn.addEventListener('click', prevTrack);
            nextBtn.addEventListener('click', nextTrack);
            
            audioPlayer.addEventListener('timeupdate', updateProgress);
            audioPlayer.addEventListener('ended', nextTrack);
            
            progressBar.addEventListener('click', setProgress);
            
            // Кнопки управления
            addTrackBtn.addEventListener('click', openModal);
            editPlaylistBtn.addEventListener('click', toggleEditMode);
            
            // Модальные окна
            closeModalBtn.addEventListener('click', closeModal);
            closeEditModalBtn.addEventListener('click', closeEditModal);
            
            submitTrackBtn.addEventListener('click', () => {
                addTrack(
                    trackUrlInput.value.trim(),
                    trackNameInput.value.trim()
                );
            });
            
            submitEditTrackBtn.addEventListener('click', saveEditedTrack);
            
            // Закрытие модальных окон по клику вне их
            addTrackModal.addEventListener('click', (e) => {
                if (e.target === addTrackModal) {
                    closeModal();
                }
            });
            
            editTrackModal.addEventListener('click', (e) => {
                if (e.target === editTrackModal) {
                    closeEditModal();
                }
            });
            
            // Добавление по Enter
            trackUrlInput.addEventListener('keypress', (e) => {
                if (e.key === 'Enter') {
                    addTrack(
                        trackUrlInput.value.trim(),
                        trackNameInput.value.trim()
                    );
                }
            });
            
            editTrackUrlInput.addEventListener('keypress', (e) => {
                if (e.key === 'Enter') {
                    saveEditedTrack();
                }
            });
            
            // Вибрация для тактильного отклика (если доступно)
            if (navigator.vibrate) {
                const buttons = document.querySelectorAll('button');
                buttons.forEach(btn => {
                    btn.addEventListener('click', () => {
                        navigator.vibrate(10);
                    });
                });
            }
        }

        // Запуск при загрузке страницы
        document.addEventListener('DOMContentLoaded', init);
        
        // Обработка видимости страницы
        document.addEventListener('visibilitychange', () => {
            if (document.hidden) {
                // Страница скрыта
            }
        });
    </script>
</body>
</html>
