## 要求
・Editor Tool上でstatic bool値を変更するコードを組んでいたがビルドの際にリロードされて初期化されるので動かない
・Start関数内で参照するプログラムもあるので、Awakeの時点で数値の更新が完了するようにしたい
・恐らくEditor上での変更がPlayモードでも引き継がれる要素(scriptable object)を使う

## 方法
・値変更がeditorからplayモードに引き継がれるのを媒体にする
・scriptable objectを思いつくけど、同じものを参照する自信がないのが不安(手違いによる移動など...)
・addressablesに関してはロードの手間が勿体無い気がする


## コード
'''
/*
 * Copyright (c) DeepNapEngine
 * https://github.com/TrueRyoB
 */
using UnityEngine;
using System;
using System.Collections;
using System.Collections.Generic;

namespace PoseQ.Config
{
    /// <summary>
    /// For a global reference in shifting to the debug mode!
    /// </summary>
    public class GameConfig : MonoBehaviour
    {
        public static bool SkipLongAnimations = false;
        public static bool ShowDebugInfo = false;
        public static bool GodModeEnabled = false;
        public static float PlayerSpeedMultiplier = 1.0f;
        public static bool AutoScreenshotOnError = false;
    }
}

'''

'''
/*
 * Copyright (c) DeepNapEngine
 * https://github.com/TrueRyoB
 */
using UnityEngine;
using UnityEditor;
using PoseQ.Config;

namespace PoseQ.Extensions.Editor
{
    /// <summary>
    /// To change the values of static variables on an inspector
    /// </summary>
    public class GameConfigEditor : EditorWindow
    {
        [MenuItem("Tools/Game Config Editor")]
        public static void ShowWindow()
        {
            GetWindow<GameConfigEditor>("Game Config");
        }

        private void OnGUI()
        {
            GUILayout.Label("Debug Settings", EditorStyles.boldLabel);

            GameConfig.SkipLongAnimations =
                EditorGUILayout.Toggle("Skip Long Animations", GameConfig.SkipLongAnimations);
            GameConfig.ShowDebugInfo = 
                EditorGUILayout.Toggle("Show Debug Info", GameConfig.ShowDebugInfo);
            GameConfig.GodModeEnabled =
                EditorGUILayout.Toggle("God Mode Enabled", GameConfig.GodModeEnabled);
            GameConfig.AutoScreenshotOnError =
                EditorGUILayout.Toggle("Auto Screenshot On Error", GameConfig.AutoScreenshotOnError);
            GameConfig.PlayerSpeedMultiplier =
                EditorGUILayout.FloatField("Player Speed Multiplier", GameConfig.PlayerSpeedMultiplier);

            if (GUILayout.Button("Reset to Defaults"))
            {
                GameConfig.SkipLongAnimations = false;
                GameConfig.ShowDebugInfo = false;
                GameConfig.GodModeEnabled = false;
                GameConfig.AutoScreenshotOnError = false;
                GameConfig.PlayerSpeedMultiplier = 1.0f;
            }
        }
    }
}

'''
