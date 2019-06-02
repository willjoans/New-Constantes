

struct UISTORYBORD {
    static let SBMain                   = UIStoryboard.init(name:"Main", bundle: nil)
    static let SBAdmin                  = UIStoryboard.init(name:"AdminDS", bundle: nil)
    static let SBTeacher                = UIStoryboard.init(name:"TeacherDS", bundle: nil)
    static let SBStudentInfo            = UIStoryboard.init(name:"StudentInfoDS", bundle: nil)
    static let SBStudentInfoDashBord    = UIStoryboard.init(name:"StudentDashBordInfo", bundle: nil)
    static let SBStudentInfoData        = UIStoryboard.init(name:"StudentStory", bundle: nil)
    static let SBBusDriver              = UIStoryboard.init(name:"BusDriver", bundle: nil)
    
    //BusDriver
}

//MARK : ImageSet
extension UIImage {
    
    func resizeImage(image: UIImage, newSize: CGSize) -> (UIImage) {
        
        let newRect = CGRect(x: 0, y: 0, width: newSize.width, height: newSize.height).integral
        UIGraphicsBeginImageContextWithOptions(newSize, false, 0)
        let context = UIGraphicsGetCurrentContext()
        
        // Set the quality level to use when rescaling
        context!.interpolationQuality = CGInterpolationQuality.default
        let flipVertical = CGAffineTransform(a: 1, b: 0, c: 0, d: -1, tx: 0, ty: newSize.height)
        
        context!.concatenate(flipVertical)
        // Draw into the context; this scales the image
        context?.draw(image.cgImage!, in: CGRect(x: 0.0,y: 0.0, width: newRect.width, height: newRect.height))
        
        let newImageRef = context!.makeImage()! as CGImage
        let newImage = UIImage(cgImage: newImageRef)
        
        // Get the resized image from the context and a UIImage
        UIGraphicsEndImageContext()
        
        return newImage
    }
}


func IS_SIMULATOR() -> Bool {
    #if targetEnvironment(simulator)
    return true
    #else
    return false
    #endif
}
//Set landscapeLeft. portrait.
func IS_LANDSCAPE() -> Bool {
    if UIDevice.current.orientation == UIDeviceOrientation.landscapeLeft {
        return true
    } else if UIDevice.current.orientation == UIDeviceOrientation.landscapeRight {
        return true
    } else if UIDevice.current.orientation == UIDeviceOrientation.portrait {
        return false
    } else if UIDevice.current.orientation == UIDeviceOrientation.portraitUpsideDown {
        return false
    }
    return false
}

// DELAY
func RUN_AFTER_DELAY(_ delay: TimeInterval, block: @escaping ()->()) {
    let time = DispatchTime.now() + Double(Int64(delay * Double(NSEC_PER_SEC))) / Double(NSEC_PER_SEC)
    DispatchQueue.main.asyncAfter(deadline: time, execute: block)
}


extension String {
    var htmlToAttributedString: NSAttributedString? {
        guard let data = data(using: .utf8) else { return NSAttributedString() }
        do {
            return try NSAttributedString(data: data, options: [.documentType: NSAttributedString.DocumentType.html, .characterEncoding:String.Encoding.utf8.rawValue], documentAttributes: nil)
        } catch {
            return NSAttributedString()
        }
    }
    var htmlToString: String {
        return htmlToAttributedString?.string ?? ""
    }
}

////USerDefinde Local Data Store

import Foundation
import UIKit
//Pref Constant
let kDeviceToken        = "DeviceToken"
let kIsLoggedIn         = "LogaIn"

let Kstudent_img_path   = "student_img_path"

class Pref {
    class func setObject(_ obj : NSObject? , forKey:String) {
        let userDefaults = UserDefaults.standard
        if obj == nil {
            userDefaults.set(nil, forKey: forKey)
            userDefaults.synchronize()
        }
        else {
            let data = NSKeyedArchiver.archivedData(withRootObject: obj!)
            userDefaults.set(data, forKey: forKey)
            userDefaults.synchronize()
            return
        }
        
    }
    
    class func setObject(_ obj : Any? , forKey:String) {
        let userDefaults = UserDefaults.standard
        
        if obj == nil {
            userDefaults.set(obj, forKey: forKey)
            userDefaults.synchronize()
            return
        }
        else if obj is NSURL {
            userDefaults.set(obj as? URL, forKey: forKey)
        }
        else {
            userDefaults.set(obj, forKey: forKey)
        }
        userDefaults.synchronize()
    }
    
    class func getObjectForKey(_ theKey:String) -> Any? {
        let userDefaults = UserDefaults.standard
        let resultObject = userDefaults.object(forKey: theKey)
        if resultObject != nil && resultObject is Data {
            return NSKeyedUnarchiver.unarchiveObject(with: resultObject as! Data) as Any?
        }
        return UserDefaults.standard.object(forKey: theKey) as Any?
    }
    class func removeObject(_ forKey:String) {
        let userDefaults = UserDefaults.standard
        userDefaults.removeObject(forKey: forKey)
        userDefaults.synchronize()
    }
}
///Color Data
import Foundation
import UIKit


//Share The Variable
let appDelegate = UIApplication.shared.delegate as! AppDelegate



//Font Set
enum Font_Poppins : String {
    case Regular    = "Poppins-Regular"
    case Light      = "Poppins-Light"
    case Medium     = "Poppins-Medium"
    case Bold       = "Poppins"
    case Semibold   = "Poppins-SemiBold"
    case Lobster    = "Lobster-Regular"
}

struct AppFont_Poppins {
    static let HelveticaNeueMedium_13     = UIFont(name: "Poppins-Regular", size: 13.0)
    static let HelveticaNeue_13           = UIFont(name: "Poppins-Light", size: 13.0)
    static let HelveticaNeue_12           = UIFont(name: "Poppins-Medium", size: 12.0)
    static let HelveticaNeue_14           = UIFont(name: "Poppins", size: 14.0)
    static let HelveticaNeue_16           = UIFont(name: "Poppins-SemiBold", size: 21.0)
    static let HelveticaNeueLight         = UIFont(name: "Lobster-Regular", size: 14.0)
    
}

enum HttpCode: String {
    case Error_100       = "100" //None
    case Error_500       = "500" //Oops something went wrong
    case Error_503       = "503" //Http Request Failur
    case Error_504       = "504" //No InternetConnection
    case Error_410       = "410" //Token Expierd
    case Error_400       = "400" //Error Message
    case Error_401       = "401" //User Not Verify
    case Error_200       = "200" //Succsessfull
    case Error_203       = "203" //Succsessfull
}


//Color Set
func RGBA(_ r:CGFloat, g:CGFloat, b:CGFloat, a:CGFloat) -> UIColor {
    return UIColor(red: r/255, green: g/255, blue: b/255, alpha: a)
}

//Color Set
let COLOR_TOOLBAR           = UIColor(hexString: "#dc143c")
let COLOR_LIGHT_RED         = UIColor(hexString: "#9C51A1")
let COLOR_SEFFREN           = UIColor(hexString: "#f37047")
let COLOR_GREEN             = UIColor(hexString: "#9ECE66")
let COLOR_PINK              = UIColor(hexString: "#D72262")
let COLOR_CLOUD             = UIColor(hexString: "#3FB4E9")
let COLOR_RED               = UIColor(hexString: "#EB4E40")
let COLOR_YELLOW            = UIColor(hexString: "#FFCB26")
let COLOR_BLUE_LIGHT        = UIColor(hexString: "#626BDF")
let COLOR_RED_DARK          = UIColor(hexString: "#FF2600")
let COLOR_DARK              = UIColor(hexString: "#9A9A9A")
let COLOR_LIGHT_DARK        = UIColor(hexString: "#CBD0D4")
let COLOR_WHITESS           = UIColor(hexString: "#EEEEEE")
let COLRO_DARKGREENSS         = UIColor(hexString: "#00F7B0")


let COLOR_BLUE_BLACKS        = UIColor(red: 0/255, green: 125/255, blue: 225/255, alpha: 0.2)
let COLOR_GREEN_BLACK   = UIColor(hexString: "#9ece66")
let COLOR_BLUE_DARK = UIColor(red: 62/255, green: 180/255, blue: 223/255, alpha: 1)
let COLOR_DARK_BLUE_GRAY = UIColor(red: 112/255, green: 124/255, blue: 210/255, alpha: 1)
let COLOR_GREEN_BG = UIColor(red: 66/255, green: 174/255, blue: 39/255, alpha: 1)
let COLOR_BLACK_BG = UIColor(red: 26/255, green: 26/255, blue: 26/255, alpha: 1)
let COLOR_BLACK_lIGHT = UIColor(red: 51/255, green: 51/255, blue: 51/255, alpha: 1)

let COLOR_BUTTON_SELECTED_Text = UIColor.white;
let COLOR_BUTTON_UNSELECTED_Text = COLOR_BLUE_LIGHT

extension UIColor { convenience init(hexString: String) { var cString:String = hexString.trimmingCharacters(in: .whitespacesAndNewlines).uppercased()
    if (cString.hasPrefix("#")) {
        cString = cString.substring(from: cString.index(cString.startIndex, offsetBy: 1))
    }
    
    var red : CGFloat = 0.0
    var green : CGFloat = 0.0
    var blue : CGFloat = 0.0
    
    if ((cString.count) != 6) {
        red = 0
        green = 0
        blue = 0
    } else {
        var rgbValue:UInt32 = 0
        Scanner(string: cString).scanHexInt32(&rgbValue)
        red = CGFloat((rgbValue & 0xFF0000) >> 16) / 255.0
        green = CGFloat((rgbValue & 0x00FF00) >> 8) / 255.0
        blue = CGFloat(rgbValue & 0x0000FF) / 255.0
    }
    self.init(red: red, green: green, blue: blue, alpha: 1.0)
    }
}
extension CALayer {
    func addBorder(_ edge: UIRectEdge, color: UIColor, thickness: CGFloat) {
        
        let border = CALayer();
        
        switch edge {
        case UIRectEdge.top:
            border.frame = CGRect(x: 0, y: 0, width: self.frame.width, height: thickness);
            break
        case UIRectEdge.bottom:
            border.frame = CGRect(x: 0, y: self.frame.height - thickness, width: self.frame.width, height: thickness)
            break
        case UIRectEdge.left:
            border.frame = CGRect(x: 0, y: 0, width: thickness, height: self.frame.height)
            break
        case UIRectEdge.right:
            border.frame = CGRect(x: self.frame.width - thickness, y: 0, width: thickness, height: self.frame.height)
            break
            // case UIRectEdge.All: // border.frame = self.bounds // break default: break }
            border.backgroundColor = color.cgColor;
            
            self.addSublayer(border)
        default:
            return
            
        }
        
    }
}
//validetion 
import Foundation
import UIKit

//Email ID
struct EmailConstance {
    static func isValidEmail(testStr:String) -> Bool {
        let emailRegEx = "[A-Z0-9a-z._%+-]+@[A-Za-z0-9.-]+\\.[A-Za-z]{2,64}"
        let emailTest = NSPredicate(format:"SELF MATCHES %@", emailRegEx)
        return emailTest.evaluate(with: testStr)
    }
}
extension String {
    func trim() -> String {
        return self.trimmingCharacters(in: CharacterSet.whitespacesAndNewlines)
    }
}
extension String {
    
    func isEmail() -> Bool {
        let regex = try? NSRegularExpression(pattern: "[A-Z0-9a-z._%+-]+@[A-Za-z0-9.-]+\\.[A-Za-z]{2,6}", options: .caseInsensitive)
        
        return regex?.firstMatch(in: self, options: [], range: NSMakeRange(0, self.characters.count)) != nil
    }
    
    func isStringWithoutSpace() -> Bool{
        return !self.trimmingCharacters(in: CharacterSet.whitespacesAndNewlines).isEmpty
    }
    
    func toDouble() -> Double? {
        return Double(self)//NSNumberFormatter().numberFromString(self)?.doubleValue
    }
    
    var floatValue: Float {
        return (self as NSString).floatValue
    }
    func isPasswordValid(_ password : String) -> Bool{
        let passwordTest = NSPredicate(format: "SELF MATCHES %@", "^(?=.*[0-9])(?=.*[a-z])(?=.*[$@$#!%*?&])[A-Za-z\\d$@$#!%*?&]{8,}")
        return passwordTest.evaluate(with: password)
    }
    
    func contains(find: String) -> Bool{
        return self.range(of:find, options:String.CompareOptions.caseInsensitive) != nil
    }
    
    
    
    func replace(string:String, replacement:String) -> String {
        return self.replacingOccurrences(of: string, with: replacement, options: .literal, range: nil)
    }
    
    func removeWhitespace() -> String {
        return self.replace(string: " ", replacement: "")
    }
    
}









