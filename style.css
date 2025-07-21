import React, { useState } from "react";
import * as XLSX from "xlsx";

export default function CRMSystem() {
  const [requests, setRequests] = useState([]);
  const [uploadedFiles, setUploadedFiles] = useState([]);
  const [selectedRequest, setSelectedRequest] = useState(null);
  const [selectedIndex, setSelectedIndex] = useState(null);
  const [extraInputs, setExtraInputs] = useState({});

  const categorizeRequest = (text) => {
    const lowered = text.toLowerCase();
    if (
      lowered.includes("شكوى") ||
      lowered.includes("تظلم") ||
      lowered.includes("اعتراض") ||
      lowered.includes("انزعاج")
    )
      return "Complaint - شكوى";
    if (
      lowered.includes("أرجو") ||
      lowered.includes("أطلب") ||
      lowered.includes("يرجى") ||
      lowered.includes("طلب")
    )
      return "Request for Service - طلب الخدمة";
    if (
      lowered.includes("بلاغ") ||
      lowered.includes("عطل") ||
      lowered.includes("مكسور") ||
      lowered.includes("خطر")
    )
      return "Incident - بلاغ";
    if (
      lowered.includes("شكر") ||
      lowered.includes("تقدير") ||
      lowered.includes("ثناء")
    )
      return "Compliment - ثناء";
    if (
      lowered.includes("استفسار") ||
      lowered.includes("معلومات") ||
      lowered.includes("أود أن أعرف")
    )
      return "Information - معلومات";
    if (lowered.includes("اقترح") || lowered.includes("اقتراح"))
      return "Suggestion - اقتراح";
    return "غير مصنف";
  };

  const getEmailTemplate = (category, extra = {}) => {
    switch (category) {
      case "Request for Service - طلب الخدمة":
        return `السلام عليكم ورحمة الله وبركاته\n\nالسيد\nتحية طيبة وبعد,\n\nيرجى التكرم ومتابعة الطلب رقم: ${extra.requestNumber || ""}.\n\nولكم جزيل الشكر`;
      case "Complaint - شكوى":
        return `السلام عليكم ورحمة الله وبركاته\n\nالسيد\nتحية طيبة وبعد,\n\nيرجى العلم بأننا تلقينا شكوى بخصوص\n\nولك جزيل الشكر`;
      case "Incident - بلاغ":
        return `السلام عليكم ورحمة الله وبركاته\n\nالمهندسين الكرام\nتحية طيبة وبعد,\n\nيرجى التكرم والإطلاع على الصور المرفقة وعمل اللازم حيث وردنا بلاغ\n\nالإحداثيات: ${extra.coords || ""}\nاسم المتعامل: ${extra.name || ""}\nرقم التواصل: ${extra.contact || ""}\n\nولكم جزيل الشكر`;
      default:
        return "نشكركم على تواصلكم، تم تسجيل طلبكم وسيتم التعامل معه من قبل الفريق المختص.";
    }
  };

  const handleUpload = (e) => {
    const file = e.target.files[0];
    if (file) {
      setUploadedFiles([...uploadedFiles, file.name]);
      const reader = new FileReader();
      reader.onload = (evt) => {
        const bstr = evt.target.result;
        const wb = XLSX.read(bstr, { type: "binary" });
        const wsname = wb.SheetNames[0];
        const ws = wb.Sheets[wsname];
        const data = XLSX.utils.sheet_to_json(ws);
        const extracted = data.map((row) => {
          const text = row["Description"] || "";
          const resolution = row["Resolution Response"] || "";
          const status = row["Status"] || "";
          const caseType = row["Case Type"] || "";

          const requestMatch = resolution.match(/\d{7,15}/);
          const extractedRequestNumber = requestMatch ? requestMatch[0] : "";

          const referenceMatch = text.match(/\d{7,15}/);
          const extractedReferenceNumber = referenceMatch
            ? referenceMatch[0]
            : "";

          const category =
            status === "Classified" ? caseType : categorizeRequest(text);
          return {
            text,
            category,
            status,
            extractedRequestNumber,
            extractedReferenceNumber,
          };
        });
        setRequests([...requests, ...extracted]);
      };
      reader.readAsBinaryString(file);
    }
  };

  const handleSendEmail = (index) => {
    setSelectedRequest(requests[index]);
    setSelectedIndex(index);
  };

  const handleClose = (index) => {
    const updated = [...requests];
    updated[index].status = "Closed";
    setRequests(updated);
  };

  const handleInputChange = (index, field, value) => {
    setExtraInputs((prev) => ({
      ...prev,
      [index]: {
        ...prev[index],
        [field]: value,
      },
    }));
  };

  return (
    <div style={{ padding: 20, fontFamily: "Arial" }}>
      <h1>📋 CRM Automation System</h1>

      <input type="file" onChange={handleUpload} />

      <h2>📁 Uploaded Files</h2>
      <ul>
        {uploadedFiles.map((file, idx) => (
          <li key={idx}>{file}</li>
        ))}
      </ul>

      <h2>📨 Requests</h2>
      <table border="1" cellPadding="10">
        <thead>
          <tr>
            <th>Text</th>
            <th>Category</th>
            <th>Status</th>
            <th>Case/Request ID - رقم الشكوى/الطلب</th>
            <th>Account/Bill No. - رقم الحساب/الفاتورة</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          {requests.map((req, idx) => (
            <React.Fragment key={idx}>
              <tr>
                <td>{req.text}</td>
                <td>{req.category}</td>
                <td>{req.status}</td>
                <td>
                  {req.extractedRequestNumber}
                  {req.extractedRequestNumber && (
                    <button
                      style={{ marginLeft: 5 }}
                      onClick={() =>
                        navigator.clipboard.writeText(
                          req.extractedRequestNumber,
                        )
                      }
                      title="Copy Request Number"
                    >
                      📋
                    </button>
                  )}
                </td>
                <td>
                  {req.extractedReferenceNumber}
                  {req.extractedReferenceNumber && (
                    <button
                      style={{ marginLeft: 5 }}
                      onClick={() =>
                        navigator.clipboard.writeText(
                          req.extractedReferenceNumber,
                        )
                      }
                      title="Copy Account/Bill No."
                    >
                      📋
                    </button>
                  )}
                </td>
                <td>
                  <button onClick={() => handleSendEmail(idx)}>
                    Send Email
                  </button>{" "}
                  <button onClick={() => handleClose(idx)}>Close</button>
                </td>
              </tr>
              {selectedIndex === idx && (
                <tr>
                  <td colSpan="6">
                    <div
                      style={{
                        border: "1px solid #ccc",
                        padding: 20,
                        marginTop: 10,
                        background: "#f9f9f9",
                      }}
                    >
                      <h3>📨 Email Preview</h3>
                      <p>
                        <strong>Category:</strong> {selectedRequest.category}
                      </p>
                      <p>
                        <strong>Status:</strong> {selectedRequest.status}
                      </p>
                      {selectedRequest.category === "Incident - بلاغ" && (
                        <>
                          <label>الإحداثيات:</label>
                          <input
                            onChange={(e) =>
                              handleInputChange(idx, "coords", e.target.value)
                            }
                          />
                          <br />
                          <label>اسم المتعامل:</label>
                          <input
                            onChange={(e) =>
                              handleInputChange(idx, "name", e.target.value)
                            }
                          />
                          <br />
                          <label>رقم التواصل:</label>
                          <input
                            onChange={(e) =>
                              handleInputChange(idx, "contact", e.target.value)
                            }
                          />
                          <br />
                        </>
                      )}
                      {selectedRequest.category ===
                        "Request for Service - طلب الخدمة" && (
                        <>
                          <label>رقم الطلب:</label>
                          <input
                            onChange={(e) =>
                              handleInputChange(
                                idx,
                                "requestNumber",
                                e.target.value,
                              )
                            }
                          />
                          <br />
                        </>
                      )}
                      <textarea
                        rows="8"
                        cols="100"
                        readOnly
                        value={getEmailTemplate(
                          selectedRequest.category,
                          extraInputs[idx],
                        )}
                      />
                      <br />
                      <button
                        onClick={() =>
                          navigator.clipboard.writeText(
                            getEmailTemplate(
                              selectedRequest.category,
                              extraInputs[idx],
                            ),
                          )
                        }
                      >
                        Copy Email
                      </button>{" "}
                      <button
                        onClick={() =>
                          setSelectedRequest(null) || setSelectedIndex(null)
                        }
                      >
                        Close
                      </button>
                    </div>
                  </td>
                </tr>
              )}
            </React.Fragment>
          ))}
        </tbody>
      </table>
    </div>
  );
}
