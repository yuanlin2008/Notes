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
# Commands
## Obj
UObject相关的命令主要在Obj.cpp的bool StaticExec( UWorld* InWorld, const TCHAR* Cmd, FOutputDevice& Ar )函数和UnrealEngine.cpp的bool UEngine::HandleObjCommand( const TCHAR* Cmd, FOutputDevice& Ar )函数中 
|命令|描述|
|-|-|
|listprops|listprops classname wildcard|
|listfuncs||
|listfunc||
