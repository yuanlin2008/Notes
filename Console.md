# Console Manager
* SConsoleInputBox::OnTextCommitted
    * FConsoleCommandExecutor::Exec
        * UUnrealEdEngine::Exec
            * UUnrealEdEngine::Exec
                * UEditorEngine::Exec
                    * UEngine::Exec
                        * FConsoleManager::ProcessUserConsoleInput

# stat
* SConsoleInputBox::OnTextCommitted
    * FConsoleCommandExecutor::Exec
        * UUnrealEdEngine::Exec
            * UUnrealEdEngine::Exec
                * UEditorEngine::Exec
                    * UEngine::Exec
                        * StaticExec
                            * FSelfRegisteringExec::StaticExec
                                * FStatCmdCore::Exec
