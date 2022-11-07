```puml
!theme black-knight

interface IStreamingManager
interface IRenderAssetStreamingManager
class FRenderAssetStreamingManager
class FRenderAssetUpdate


IStreamingManager<|--IRenderAssetStreamingManager
IRenderAssetStreamingManager<|--FRenderAssetStreamingManager

```